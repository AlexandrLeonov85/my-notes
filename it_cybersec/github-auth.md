Авторизация на GitHub по паролю более недоступна. Я решил этот вопрос установкой github-cli:

```
$ sudo pacman -Syu github-cli
$ gh auth login
$ gh auth setup-git
```

Работа этих команд происходит в диалоговом режиме, можно легко разобраться.
[Documentation](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github)

---
###### При попытке запушить на сервер изменения, получил такую вот ошибочку:
```Перечисление объектов: 10, готово.  
remote: error: GH007: Your push would publish a private email address.  
remote: You can make your email public or disable this protection by visiting:  
remote: http://github.com/settings/emails  
To https://github.com/AlexandrLeonov85/learning-git.git  
! [remote rejected] my-notes -> my-notes (push declined due to email privacy restrictions)  
error: не удалось отправить некоторые ссылки в «https://github.com/AlexandrLeonov85/learning-git.git»
```
[Решение](https://stackoverflow.com/questions/43863522/error-your-push-would-publish-a-private-email-address) в следующем: надо сменить email на адрес вида '107890037+AlexandrLeonov85@users.noreply.github.com' в настройках git, его можно взять в настройках GitHub: Settings → Emails, под чекбоксом 'Keep my email addresses private'.
Либо можно просто снять галку в вышеупомянутом чекбоксе.