(以前的笔记了，整理仓库分到这来)
# 如何从git上传文件到github
 How to upload files from git to github 


# 1、  输入自己的用户名和邮箱（为注册GITHUB账号时的用户名和邮箱）

    $ git config --global user.name "github账号名" 
    $ git config --global user.email "github邮箱"
    
    
# 2、设置SSH key
   
    $ cd ~/.ssh // 检查是否有ssh密钥
    $ ls // 显示文件
    
   如果什么都没有输出 则说明没有密钥
   
    $ ssh-keygen -t rsa -C "github账号名" // 生成密钥
    $ cat id_rsa.pub // 读取密钥 把密钥显示出来
    
   这时候就可以复制生成的密钥了
   
   找到github路径 settings > Deploy keys > add deploy key
   
   随便起个title，然后将复制的密钥粘贴在下面的输入框 保存即可
   
# 3、复制仓库地址

   此时在github该仓库下 点击"clone or download" 复制仓库地址 如： https://github.com/KamyoChae/ColorFindFine.git
   
# 4、初始化git及上传文件

   在需要上传的文件目录下面 使用  
     
     $  git init // 初始化git
     
     $ git add .         //添加当前文件夹下的所有文件
     
   （如果想单独上传一个文件可以输入这一句：$ git add xx.css   //添加当前文件夹下的xx.css这个文件）
     
     $ git commit -m "v1.0.0 from kamyochae"  //引号中的内容为对该文件的描述
     
     $ git remote add origin https://github.com/KamyoChae/ColorFindFine.git // 告诉git,我要传到这个仓库里面了
     
   （如果出现错误：fatal: remote origin already exists，则执行以下语句：$ git remote rm origin）
     
     $ git pull --rebase origin master // 和仓库同步一下 把远程服务器github上面的文件拉下来
     
 # 5、完成
 
     $ git push origin master // 大功告成
     
# 刷新github就可以看到上传的文件了
