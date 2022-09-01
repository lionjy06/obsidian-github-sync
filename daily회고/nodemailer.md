NestJs를 이용하여 회원가입을 완료한 유저에게 welcome 메일을 보내는 api를 만들었다. 이때 nodemailer라는 라이브러리를 사용하였다. 

필요한 패키지
```
 yarn add @nestjs-modules/mailer nodemailer handlebars @types/nodemailer
```

패키지를 설치한후 
```
mail.service.ts

import { MailerService } from '@nestjs-modules/mailer';
import { Injectable } from '@nestjs/common';
import * as nodemailer from 'nodemailer';

@Injectable()

export class MailService {
constructor(private readonly mailerService: MailerService) {}

async sendMail(email, name) {
try {

const transporter = nodemailer.createTransport({
service: 'gmail',
host: 'smtp.gmail.com',
port: 587,
secure: false,
auth: {
user: process.env.NODEMAILER_USER,
pass: process.env.NODEMAILER_PASS,

	},

});

  

transporter.sendMail({
to: email,
from: 'jyjjyj06@gmail.com',
subject: `${name}님 가입을 축하드립니다.`,
html: `${name}님 가입을 축하드립니다! 앞으로 최선의 서비스로 모시겠습니다.`,
});

return '이메일 발송 성공';

} catch (err) {
console.log(err);
return false;

		}

	}

}
```
transporter라는 변수안에 nodemailer.createTransport로 보내는 즉 나의 정보를 입력해준다. 이후 sendMail을 통해 보내고자 하는 객체와 내용을 실어보낸다.

```
user.controller.ts

@Post('create')
async createUser(
@Body() createUserDto: CreateUserDto,
@Res() response: Response,
) {

const { password, phoneNumber, name, email, ...rest } = createUserDto;

  
const hashedPassword = await bcrypt.hash(password, 5);
await this.usersService.createUser({
...rest,
hashedPassword,
phoneNumber,
name,
email,
});

await this.mailService.sendMail(email, name);

return response.status(201).json({
status: 200,
statusName: '회원가입이 성공적으로 완료되었습니다.',
	});

}
```
mail.service.ts로 부터 sendMail을 inject받아 회원가입 데이터 저장이 완료되면 환영메일 발송을 하게되는 로직을 짯다.


총평: 진짜 별거없는데 괜히 nestJs에서 제공하는 MailerModule이랑 혼용하다가 하루 꼬박 넘겨서야 완성이 되었다.   

마주친 에러:
1. Missing credential for 'plain' : nodemailer.createTransport에서 auth에 유저이메일과 유저패스워드만 잘 입력해도 해결된다.

2. cannot reat property of null (createTransport can't read undefined): 이게 처음에 nodemailer를 import 해올때 import nodemailer from "nodemailer"로 받아왔는데 이런에러가 떳었다. 그래서 구글링해서 찾아보니까 import * as nodemailer from "nodemailer"로 바꾸면 해결된다고 해서 바꿔봤더니 진짜 해결됫다.