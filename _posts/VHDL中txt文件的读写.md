&emsp;&emsp;在对VHDL代码进行ModelSim仿真的时候，如果测试一个比较简单的功能，比如简单地测试一个IPCore，那么我们只需要
```VHDL
signalName <= x"01"; wait for cam_period*5; 
signalName <= x"10"; wait for cam_period*5;
```
&emsp;&emsp;类似的代码就可以满足我们的要求。

&emsp;&emsp;但是呢，假如你要测试一个大的component，或者是测试一个数据序列，抑或是一个图像，那么这时候，通过文件读取数据将会使工作简单很多。与此同时，为了保存试验结果，也常常用到文件。尤其是对于FPGA的图像处理，文件读取必不可少。首先将一副正常图像转换为一个txt文件，然后作为FPGA的输入，经过处理的图像数据再保存成txt文件，再将这个txt文件转换为图像，这样就可以直观地看到图像处理的效果了。

&emsp;&emsp;那么今天笔者就来讲一下VHDL中txt文件的读取和写入。
```VHDL
file_open(file_status,FILE_OUT,"data_record.txt",write_mode);
```
&emsp;&emsp;上面那句代码是整个文件读取与写入系统中最重要的，意思是以`write Mode`形式打开`data_record.txt`这个文档。

&emsp;&emsp;txt文本的读写方式一共有三种，分别是`read_mode`、`write_mode`和`append_mode`。按照字面意思就可以理解，分别是读取模式、写入模式与扩展模式。`append_mode`与`write_mode`的区别在于，`write_mode`会清除掉文本中原来存在的内容，而`append_mode`不会。
```VHDL
file_open(file_status,FILE_OUT,"data_record.txt",read_mode);
file_open(file_status,FILE_OUT,"data_record.txt",append_mode);
```
&emsp;&emsp;这是另外两种模式。
```VHDL
--读取数据
stim_process : process(cam_clk)
variable i : integer:= 0;
file TEST_IN  : TEXT;
variable LINE_IN: line;
variable dat_in : std_logic_vector(31 downto 0);
begin
  if(rst_n = '0' and dataIn = '0') then
    file_open(TEST_IN, "image-binary-data.txt", READ_MODE);
    dataIn <= '1';
  elsif(rising_edge(cam_clk)) then
    if((i mod ((512*135) + 300)) < 300) then
      vs_i <= '0';
    else
      vs_i <= '1';
    end if;
    if (((i mod ((512*135) + 300)) < 300) or (((i mod (512*135+300))-300) mod 135) < 7) then
      hs_i <= '0';
      dv_i <= '0';
      da_i <= (others => '0');
    else
      hs_i <= '1';
      dv_i <= '1';
      readline(TEST_IN, LINE_IN);
      read(LINE_IN, dat_in); 
      da_i <= dat_in;
    end if;
    i := i + 1;
  end if;
end process;
```
&emsp;&emsp;这是从`image-binary-data.txt`读取数据的一个过程，当然其中还包含了一个判断的过程，只在某些时刻才会从文本中读取数据。

&emsp;&emsp;在这边有一个小技巧，就是设置一个标志signal，这里我设置的是`dataIn`，设置其初始值为`'0'`，当文本打开之后，将该signal设置为`'1'`，在`rst_n = '0'`以及`dataIn = '0'`这两个条件都满足的情况下才进行文本的打开，之后就开始进行文本按行地读取，并输入到下一个component中。

&emsp;&emsp;下面介绍一下文本的写入。
```VHDL
--保存输出数据
process(cam_clk)
FILE FILE_OUT : TEXT;
variable file_status:file_open_status;
variable buf:LINE;
begin
   if(rst_n = '0' and dataOut = '0') then
      file_open(file_status,FILE_OUT,"data_record.txt",write_mode);
	  file_close(FILE_OUT);
      file_open(file_status,FILE_OUT,"data_record.txt",append_mode);
	  dataOut <= '1';
   elsif(rising_edge(cam_clk))then
	 if(dv_o='1') then
	   write(buf,da_o);
	   writeline(FILE_OUT,buf);
	 end if;
	 if(sim_end = '1') then
	   file_close(FILE_OUT);
	 end if;
   end if;
end process;
```
&emsp;&emsp;和上面的代码一样，这里也有一个dataOut的标志signal，在`rst_n = '0' and dataOut = '0'`这两个要求同时满足的时候才打开文本。但是看到在`rst_n = '0' and dataOut = '0'`这个条件下面做了四件事情，打开文本，再关闭，再打开，然后将标志signal置'1'，而且两次打开的模式不同，一次是`write_mode`，一次是`append_mode`，为什么要这样处理呢。下面给大家讲一下这个缘由。

&emsp;&emsp;文本打开了，肯定是要关闭的，那如果用write_mode这种模式打开的话，`file_close(FILE_OUT)`的时候，也就是文本关闭的时候，文本中只会留下一行数据。而换成`append_mode`这种模式的话，在`file_close(FILE_OUT)`的时候，之前写入文本的数据都会留下来。那为什么先要用`write_mode`这种模式打开呢，我用这种模式打开了，然后再关闭，整个文档里的内容就清除了，就可以保证接下去操作的是空文本，这就是先用`write_mode`打开的原因。 

&emsp;&emsp;要注意的是，在写入的状态下，在没有关闭文本的情况下，文本的大小一直是0K，但是呢，打开还是能看到文本中的数据信息的，里面的数据也可以复制出来，但是这些数据并不真正存在于这个文本中，这个文本也被ModelSim占用，不能复制，剪切等操作。

&emsp;&emsp;这里设置了一个结束仿真的标志signal`sim_end`，这个默认值也是`'0'`。有两种用法，一个是在仿真代码里写明什么情况这个signal置`'1'`，比如延时多少之后，或者是某个标志位变化了之类的。还有一种比较暴力，就是利用ModelSim中signal的Force，觉得仿真了一段时间差不多了，就将该值强制Force为'1'，然后文本就关闭了，仿真就结束了。

&emsp;&emsp;在`write_mode`和`append_mode`的时候，要写入的文本是会自动生成的，不需要事先生成，这样就比较方便，比如说仿真的时候，把输出文档命名成时间，或者是跟更改参数有关的文件名，多仿真几次之后再去处理这些生成的文本。

&emsp;&emsp;要提醒的是，输入进来的数据和输出的数据都是二进制数据，所以要先用Matlab或者是Visual Studio等工具先将数据转换成二进制数据。同理，输出的数据也要先转换为普通数据，才能很好地去观察其特性。
