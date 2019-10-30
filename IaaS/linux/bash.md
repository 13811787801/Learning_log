19.10.3
Hello everone today about Bashhhhhhh for Linu 

bash 是 Linux预设的 shell 库

## Bash 提供的功能
### 历史命令行  (History)
bash提供了记录使用过的指令，通过上下键，可以找到执行过的命令  
记录在 *～/.bash_history* 如下图

![](https://user-gold-cdn.xitu.io/2019/10/3/16d8fdca982cbecd?w=1060&h=714&f=png&s=201552)

### 命令與檔案补全功能： 
這個按鍵的功能就是在 bash 裡頭才有的！
常常在 bash 環境中使用 [tab] 是個很棒的習慣喔！因為至少可以讓你 
1. 少打很多字 
2. 確定輸入的資料是正確的

### 工作控制

### 程序化脚本 Shell Script
就是将一连串的命令行，保存到一个可执行文件，减少工作


### 詢指令是否為 Bash shell 的內建命令：type
> 選項與參數：  
    不加任何選項與參數時，type 會顯示出 name 是外部指令還是 bash 內建指令   
-t  ：當加入 -t 參數時，type 會將 name 以底下這些字眼顯示出他的意義：
      file    ：表示為外部指令
      alias   ：表示該指令為命令別名所設定的名稱
      builtin ：表示該指令為 bash 內建的指令功能  
-p  ：如果後面接的 name 為外部指令時，才會顯示完整檔名    
-a  ：會由 PATH 變數定義的路徑中，將所有含 name 的指令都列出來，包含 alias

