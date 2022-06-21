### Пробуем работать с git

Проверяем, как работает аутентификация (github.com больше не пускает по паролю, он просит авторизироваться через personal access token https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
--- проверка не прошла, запросил пароль второй раз.

`$ git remote set-url origin https://username:<MYTOKEN>@github.com/scuzzlebuzzle/ol3-1.git` — пробуем с такой командой.