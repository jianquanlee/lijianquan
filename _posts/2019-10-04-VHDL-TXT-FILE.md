---
layout: post
title: VHDL��дTXT�ļ�
date: 2019-10-04
Author: Jianquan Li
tags: [VHDL, FPGA]
comments: true
---

VHDL��дTXT�ļ�
<!-- <font face = "����" size = 4>�����ڶ�<font face = "Times New Roman">VHDL</font>�������<font face = "Times New Roman">ModelSim</font>�����ʱ���������һ���Ƚϼ򵥵Ĺ��ܣ�����򵥵ز���һ��<font face = "Times New Roman">IPCore</font>����ô����ֻ��Ҫ</font>
```
signalName <= x"01"; wait for cam_period*5; 
signalName <= x"10"; wait for cam_period*5;
```
<font face = "����" size = 4>���ƵĴ���Ϳ����������ǵ�Ҫ��</font>
<font face = "����" size = 4>���������أ�������Ҫ����һ�����<font face = "Times New Roman">COMPONENT</font>�������ǲ���һ���������У��ֻ���һ��ͼ����ô��ʱ��ͨ���ļ���ȡ���ݽ���ʹ�����򵥺ܶࡣ���ͬʱ��Ϊ�˱�����������Ҳ�����õ��ļ��������Ƕ���<font face = "Times New Roman">FPGA</font>��ͼ�����ļ���ȡ�ز����١����Ƚ�һ������ͼ��ת��Ϊһ��<font face = "Times New Roman">txt</font>�ļ���Ȼ����Ϊ<font face = "Times New Roman">FPGA</font>�����룬���������ͼ�������ٱ����<font face = "Times New Roman">txt</font>�ļ����ٽ����<font face = "Times New Roman">txt</font>�ļ�ת��Ϊͼ�������Ϳ���ֱ�۵ؿ���ͼ�����Ч���ˡ�</font>
<font face = "����" size = 4>������ô������߾�����һ��<font face = "Times New Roman">VHDL</font>��<font face = "Times New Roman">txt</font>�ļ��Ķ�ȡ��д�롣
```
file_open(file_status,FILE_OUT,"data_record.txt",write_mode);
```
<font face = "����" size = 4>���������Ǿ�����������ļ���ȡ��д��ϵͳ������Ҫ�ģ���˼����`write Mode`��ʽ��<font face = "Times New Roman">��data_record.txt��</font>����ĵ���</font>
<font face = "����" size = 4>����<font face = "Times New Roman">TXT</font>�ı��Ķ�д��ʽһ�������֣��ֱ���`READ_MODE`��`WRITE_MODE`��`APPEND_MODE`������������˼�Ϳ�����⣬�ֱ��Ƕ�ȡģʽ��д��ģʽ����չģʽ��`APPEND_MODE`��`WRITE_MODE`���������ڣ�`WRITE_MODE`��������ı���ԭ�����ڵ����ݣ���`APPEND_MODE`���ᡣ</font>
```
file_open(file_status,FILE_OUT,"data_record.txt",read_mode);
file_open(file_status,FILE_OUT,"data_record.txt",append_mode);
```
<font face = "����" size = 4>������������ģʽ��</font>
```
--��ȡ����
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
<font face = "����" size = 4>�������Ǵ� <font face = "Times New Roman">image-binary-data.txt</font> ��ȡ���ݵ�һ�����̣���Ȼ���л�������һ���жϵĹ��̣�ֻ��ĳЩʱ�̲Ż���ı��ж�ȡ���ݡ�</font>
<font face = "����" size = 4>�����������һ��С���ɣ���������һ����־<font face = "Times New Roman">signal</font>�����������õ��� `dataIn` , �������ʼֵΪ`'0'`�����ı���֮�󣬽���<font face = "Times New Roman">signal</font>����Ϊ'1'���� `rst_n = '0'`�Լ�`dataIn = '0'` ���������������������²Ž����ı��Ĵ򿪣�֮��Ϳ�ʼ�����ı����еض�ȡ�������뵽��һ��<font face = "Times New Roman">COMPONENT</font>�С�</font>
<font face = "����" size = 4>�����������һ���ı���д�롣</font>
```VHDL
--�����������
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
<font face = "����" size = 4>����������Ĵ���һ��������Ҳ��һ��<font face = "Times New Roman">dataOut</font>�ı�־<font face = "Times New Roman">signal</font>����`rst_n = '0' and dataOut = '0'` ������Ҫ��ͬʱ�����ʱ��Ŵ��ı������ǿ����� `rst_n = '0' and dataOut = '0'` ����������������ļ����飬���ı����ٹرգ��ٴ򿪣�Ȼ�󽫱�־<font face = "Times New Roman">signal</font>��<font face = "Times New Roman">'1'</font>���������δ򿪵�ģʽ��ͬ��һ����`write_mode`��һ����`append_mode`��ΪʲôҪ���������ء��������ҽ�һ�����Ե�ɡ�</font>
<font face = "����" size = 4>�����ı����ˣ��϶���Ҫ�رյģ��������<font face = "Times New Roman">write_mode</font>����ģʽ�򿪵Ļ���<font face = "Times New Roman">file_close(FILE_OUT)</font>��ʱ��Ҳ�����ı��رյ�ʱ���ı���ֻ������һ�����ݡ�������<font face = "Times New Roman">append_mode</font>����ģʽ�Ļ�����<font face = "Times New Roman">file_close(FILE_OUT)</font>��ʱ��֮ǰд���ı������ݶ�������������Ϊʲô��Ҫ��<font face = "Times New Roman">write_mode</font>����ģʽ���أ���������ģʽ���ˣ�Ȼ���ٹرգ������ĵ�������ݾ�����ˣ��Ϳ��Ա�֤����ȥ�������ǿ��ı������������<font face = "Times New Roman">write_mode</font>�򿪵�ԭ��</font>
<font face = "����" size = 4>����Ҫע����ǣ���д���״̬�£���û�йر��ı�������£��ı��Ĵ�Сһֱ��<font face = "Times New Roman">0K</font>�������أ��򿪻����ܿ����ı��е�������Ϣ�ģ����������Ҳ���Ը��Ƴ�����������Щ���ݲ�����������������ı��У�����ı�Ҳ��<font face = "Times New Roman">ModelSim</font>ռ�ã����ܸ��ƣ����еȲ�����</font>
<font face = "����" size = 4>��������������һ����������ı�־<font face = "Times New Roman">signal </font>`sim_end`�����Ĭ��ֵҲ��<font face = "Times New Roman">'0'</font>���������÷���һ�����ڷ��������д��ʲô������<font face = "Times New Roman">signal</font>��һ��������ʱ����֮�󣬻�����ĳ����־λ�仯��֮��ġ�����һ�ֱȽϱ�������������<font face = "Times New Roman">ModelSim</font>��<font face = "Times New Roman">signal</font>��<font face = "Times New Roman">Force</font>�����÷�����һ��ʱ�����ˣ��ͽ���ֵǿ��<font face = "Times New Roman">Force</font>Ϊ<font face = "Times New Roman">'1'</font>��Ȼ���ı��͹ر��ˣ�����ͽ����ˡ�</font>
<font face = "����" size = 4>������`write_mode`��`append_mode`��ʱ��Ҫд����ı��ǻ��Զ����ɵģ�����Ҫ�������ɣ������ͱȽϷ��㣬����˵�����ʱ�򣬰�����ĵ�������ʱ�䣬�����Ǹ����Ĳ����йص��ļ���������漸��֮����ȥ������Щ���ɵ��ı���</font>
<font face = "����" size = 4>����Ҫ���ѵ��ǣ�������������ݺ���������ݶ��Ƕ��������ݣ�����Ҫ����<font face = "Times New Roman">Matlab</font>������<font face = "Times New Roman">Visual Studio</font>�ȹ����Ƚ�����ת���ɶ��������ݡ�ͬ�����������ҲҪ��ת��Ϊ��ͨ���ݣ����ܺܺõ�ȥ�۲������ԡ�</font>
<center>
<font face="����" size = 5 color = "red">**��������������¶����а�����֧��һ������~**</font>
<img src="http://img.blog.csdn.net/20171120085143421" width="30%" alt=""/>
<center> -->