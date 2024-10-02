Для управления версиями node.js удобно пользоваться nvm.

```bash
brew install nvm


//После этого прописываем в PATH
source $(brew --prefix nvm)/nvm.sh

//Получаем список доступных версий
nvm ls-remote

//устанавливаем нужную версию 
nvm install node 18

//устанавливаем версию по умолчанию, если у нас их несколько
nvm alias default 16
```