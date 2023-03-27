---
title: Nodejs发送邮件
date: 2020-12-13 21:17:09
tags:
---

一、安装依赖
```nginx
npm install nodemailer
```

二、使用方法
```javascript
const nodemailer = require('nodemailer');

async send({ to, subject, text, html, filename, path, headers }) {
  const config = {
    user: '', // email
    pass: '', // 如果是qq邮箱则为smtp授权码
  };

  const transporter = nodemailer.createTransport({
    host: 'smtp.163.com',
    secureConnection: true,
    port: 465,
    secure: true,
    auth: {
      user: config.user,
      pass: config.pass,
    },
  });

  const mailOptions = {
    from: config.user,
    to,
    subject,
    text,
    html,
    attachments: [{
      filename,
      path,
    }],
    headers,
  };

  return await transporter.sendMail(mailOptions, (error, data) => {
    transporter.close();
    if (error) {
      return console.log(error);
    }
    return data;
  });
}
```

> nodemailer有时会报554错误，是因为发送的邮件内容包含了未被许可的信息，或被系统识别为垃圾邮件。

> 解决方案：subject 和 text 参数写的真实一点，不带'测试'等常见文字，增加 headers 参数，这样就可以有效避免554错误。

>  此外，在部署的时候发现163的账号发送邮件偶尔会有网络问题，导致在控制台中看到发送成功，但是收不到邮件。推荐使用QQ SMTP服务，相比163更稳定。

```javascript
await this.service.email.send({
  to: query.email,
  subject: `subject`,
  text: `text`,
  filename: 'studyData.zip',
  path: filepath,
  headers: {
    time: new Date().getTime(),
  },
});
```
