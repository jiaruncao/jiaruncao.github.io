---
layout: page
title: 关于我 
---

* 在读硕士，fall2019 USA 博士申请ing...    
* 一个普通的科研工作者，研究方向：深度学习 and 自然语言处理    
* 每一个平凡的日子里，希望能记录一些点点滴滴    
* 爱钢琴，爱游泳，爱身边人，也爱你❤️    

<p>

<h3> test </h3>  

> * 整理知识，学习笔记
> * 发布日记，杂文，所见所想
> * 撰写发布技术文稿（代码支持）
> * 撰写发布学术论文（LaTeX 公式支持）


<h3> 我的博客 </h3>  
<p>

放一段最近很喜欢的话，与君共勉


     
          
**你背单词时**                    
  
**阿拉斯加的鳕鱼正跃出水面**                    

**你算数学时**      

**太平洋彼岸的海鸥振翅掠过城市上空**       

**你晚自习时**    

**极圈中的夜空散漫了五彩斑斓**       

**但是少年你别着急**       

**在你为自己未来踏踏实实地努力时**      

**那些你感觉从来不会看到的景色**       

**那些你觉得终身不会遇到的人**       

**正一步步向你走来**      
 



### python发送邮件     

我使用的是SMTP进行邮件发送的，SMTP是发送邮件的协议，Python内置对SMTP的支持，可以发送纯文本邮件、HTML邮件以及带附件的邮件。     

Python对SMTP支持有smtplib和email两个模块，email负责构造邮件，smtplib负责发送邮件，具体代码如下： 


	from email import encoders
	from email.header import Header
	from email.mime.text import MIMEText
	from email.utils import parseaddr, formataddr
	import smtplib

	def format_addr(self,s):
	    name, addr = parseaddr(s)
	    return formataddr(( \
	        Header(name, 'utf-8').encode(), \
	        addr.encode('utf-8') if isinstance(addr, unicode) else addr))

	def send_mail(self, mail, message, title):
		from_addr = 'leopardpan@163.com'
		password = ''
		to_addr = mail
		smtp_server = 'smtp.163.com'

		msg = MIMEText(message, 'plain', 'utf-8')
		msg['From'] = self.format_addr(u'自动化测试邮件 <%s>' % from_addr)
		msg['To'] = self.format_addr(u'管理员 <%s>' % to_addr)
		msg['Subject'] = Header(title, 'utf-8').encode()

		server = smtplib.SMTP(smtp_server, 25)
		server.set_debuglevel(1)
		server.login(from_addr, password)
		server.sendmail(from_addr, [to_addr], msg.as_string())
		server.quit()

	send_mail('leopardpan@icloud.com','正文','标题')


from_addr是发送方的邮箱地址，password是开通SMTP时输入的密码     
smtp_server是smtp的服务，如果你的from_addr是gamil.com，那么就要写成smtp_server = 'smtp.gmail.com' 了。

方法 send_mail(self, mail, message, title): 有四个参数，第一个不用传，第二个参数是收信人的邮箱，第三个是邮件的正文，第四个是邮件的标题，方法调用格式： `send_mail('leopardpan@icloud.com','正文','标题')`

注意：发送方的邮箱必须要开通SMTP功能才行，否则会报错

>* SMTPSenderRefused: (550, 'User has no permission', 'leopardpan@163.com')

163的SMTP开通，需要你登录网易邮箱，然后点击顶部的设置就会出现`POP3/SMTP/IMAP`，点击之后，勾选选择开启，这个时候需要你输入密码，记住这个密码就是上面代码中的`password`，如果你都完成的话，你把上面的代码拷贝出现，把邮箱修改成你自己的，使用 pyhton 运行一下吧。


上面的几个流程结合起来就可以实现一个简单的自动化测试了，如果你有什么建议和意见欢迎讨论。


参考链接：
[SMTP发送邮件](http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001386832745198026a685614e7462fb57dbf733cc9f3ad000)     

<br>

转载请注明：[潘柏信的博客](http://baixin) » [点击阅读原文](http://baixin.io/2016/08/PythonTestAutomationiOS/) 

 
