# Setup môi trường (MacOS)

## 1. Oh My Zsh
Install command
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## 2. Homebrew
Install command
  ```
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  ```
Đối với máy sử dụng chip Apple M1, sau khi install thực hiện bổ sung thêm biến môi trường cho Homebrew bằng command sau
  ```
  echo 'eval $(/opt/homebrew/bin/brew shellenv)' >> ~/.zshrc
  ```

  > File .zshrc có thể tự tạo hoặc install Oh My Zsh trước khi run command này
  
## 3. Java
Install command (ở đây sử dụng JDK version 11)
  ```
  brew install openjdk@11
  ```

Set biến môi trường cho Java
  ``` 
  sudo ln -sfn /opt/homebrew/Cellar/openjdk@11/11.0.17/libexec/openjdk.jdk /Library/Java/JavaVirtualMachines/openjdk.jdk
  ```
## 4. Eclipse
Install command
  ```
  brew install --cask eclipse-jee
  ```

Sign eclipse command
  ```
  sudo codesign --force --deep --sign - "/Applications/Eclipse JEE.app"
  ```
## 5. Lombok
Install lombok jar: https://projectlombok.org/download
Setup command
  ```
  echo '-javaagent:/Users/hoangnhat/Documents/Workspace/env/lib/lombok.jar' >> "/Applications/Eclipse JEE.app/Contents/Eclipse/eclipse.ini"
  ```

Trong đó:
  - **/Users/hoangnhat/Documents/Workspace/env/lib/lombok.jar**: path file lombok.jar
  - **/Applications/Eclipse JEE.app/Contents/Eclipse/eclipse.ini**: path file eclipse.ini
