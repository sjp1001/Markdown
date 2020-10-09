# PYQT5

***

## pyqt5的安装与使用

- `pip install PyQt5`安装pyqt5工具

- `pip install PyQt5-tools`安装Qt Designer图形界面开发工具

  Qtdesigner在`Lib\site-packages\pyqt5_tools\Qt\bin`路径下

  (如果出现启动失败，将pyqt5_tools里面qt/plugins/platforms目录粘贴到与qt/bin/desinger.exe文件同级就解决了)
  
  **在 \Scripts\designer.exe 路径下也有！**

## QtDesinger使用

- ui转py

  - `pyuic5 $FileName$ -o ui_$FileNameWithoutExtension$.py -x`

    带 -x 可以直接跑（带\__main__运行程序）

  - 安装位置 \Scripts\pyuic5.exe

- qrc资源转换

  - `pyrcc5 $FileName$ -o $FileNameWithoutExtension$_rc.py`
  - 安装位置 \Scripts\pyrcc5.exe

- 外部工具设置（Extermal Tools）

  - 使用方法：

    ![pyqt5_7](pyqt5库\pyqt5_7.png)

    - Tools -> Extermal Tools -> Qt设计师（这名是你自己设的）

  - 设置方法：

    - File -> setting -> Tools -> External Tools -> 点 + 号
      ![pyqt5_6](pyqt5库\pyqt5_6.png)

      其中：

      - Name：是你自己设置的名字，能直接看到，比如Qt设计师

      - Description：描述，可以不写

      - Program：程序的位置（比如\Scripts\pyuic5.exe）

      - Arguments：参数（命令行除了.exe后边的所有东西）

        eg. UI转py`pyuic5 $FileName$ -o ui_$FileNameWithoutExtension$.py -x`

        要加入`$FileName$ -o ui_$FileNameWithoutExtension$.py -x`参数，不然他只启动exe文件，不会自动生成
      
      - Working directory：文件输出位置，不建议直接设置绝对路径，用宏设置相对路径
      
        点击右边 Insert Macro... 有个ProjectFileDir或者FileDir可以动态改变路径（建议用FileDir当前文件夹的上一级，保存到别的地方可以自己选）
## 基础使用

- 基础代码结构：

  ```python
  from PyQt5.Qt import *
  import sys
  
  # 创建一个应用程序对象
  app = QApplication(sys.argv)
  
  # 控件的操作
  window = QWidget()
  window.resize(500,300)
  # 添加控件
  label = QLabel(window)
  label.setText('哈哈哈')
  label.move(100,100)
  # 展示控件
  window.show()
  
  # 退出程序
  sys.exit(app.exec_())
  ```

- 面向对象进阶：

  ```python
  from PyQt5.Qt import *
  import sys
  
  class Window(QWidget):
      def __init__(self):
          super().__init__()
          self.resize(500,300)
          self.setup_ui()
  
      def setup_ui(self):
          label = QLabel(self)
          label.setText('哈哈哈')
          label.move(100, 100)
  
  if __name__ == "__main__":
      app = QApplication(sys.argv)
      window = Window()
      window.show()
      sys.exit(app.exec_())
  ```

- 模板快速导入（多继承）

  ```python
  from PyQt5.Qt import *
  # Ui_mainWindow 是UI文件自动生成的
  from test_qt.ui_huxiangguan import Ui_mainWindow
  import sys
  
  # 多继承，因为Ui_mainWindow是QMainWindow文件，所以第一个继承是QMainWindow
  class Window(QMainWindow,Ui_mainWindow):
      def __init__(self):
          super().__init__()
          # setupUi和retranslateUi是Ui_mainWindow中的方法，生成控件布局啥的
          self.setupUi(self)
          self.retranslateUi(self)
  
  if __name__ == "__main__":
      app = QApplication(sys.argv)
      window = Window()
      window.show()
      sys.exit(app.exec_())
  ```

- 给控件设置父类，则此控件在另一个控件中，语句`setparent()`

  如果不设置父类，则默认为顶层窗口

  - 可以用`setParent(父类)`设置父类

  - 也可以初始化的时候设置父类，`b = QPushButton(父类)`