

from PyQt5.QtCore import QTime


win_x, win_y = 100, 100
win_width, win_height = 1000, 600

txt_hello = 'Добро пожаловать в программу по определению состояния здоровья!'
txt_next = 'Начать'
txt_instruction = ('Данное приложение позволит вам с помощью теста Руфье провести первичную диагностику вашего здоровья.\n'
                   'Проба Руфье представляет собой нагрузочный комплекс, предназначенный для оценки работоспособности сердца при физической нагрузке.\n'
                   'У испытуемого, находящегося в положении лёжа на спине в течение 5 мин, определяют частоту пульса за 15 секунд;\n'
                   'затем в течение 45 секунд испытуемый выполняет 30 приседаний.\n'
                   'После окончания нагрузки испытуемый ложится, и у него вновь подсчитывается число пульсаций за первые 15 секунд,\n'
                   'а потом — за последние 15 секунд первой минуты периода восстановления.\n'
                   'Важно! Если в процессе проведения испытания вы почувствуете себя плохо (появится головокружение, шум в\n'
                   'ушах, сильная одышка и др.), то тест необходимо прервать и обратиться к врачу.' )
txt_title = 'Здоровье'
txt_name = 'Введите Ф.И.О.:'
txt_hintname = "Ф.И.О."
txt_hintage = "0"
txt_test1 = 'Лягте на спину и замерьте пульс за 15 секунд. Нажмите кнопку "Начать первый тест", чтобы запустить таймер.\nРезультат запишите в соответствующее поле.'
txt_test2 = 'Выполните 30 приседаний за 45 секунд. Для этого нажмите кнопку "Начать делать приседания",\nчтобы запустить счётчик приседаний.'
txt_test3 = 'Лягте на спину и замерьте пульс сначала за первые 15 секунд минуты, затем за последние 15 секунд.\nНажмите кнопку "Начать финальный тест", чтобы запустить таймер.\nЗелёным обозначены секунды, в течение которых необходимо \nпроводить измерения, черным - секунды без замера пульсаций. Результаты запишите в соответствующие поля.'
txt_sendresults = 'Отправить результаты'
txt_hinttest1 = '0'
txt_hinttest2 = '0'
txt_hinttest3 = '0'
txt_starttest1 = 'Начать первый тест'
txt_starttest2 = 'Начать делать приседания'
txt_starttest3 = 'Начать финальный тест'
time = QTime(0, 0, 15)
txt_timer = time.toString("hh:mm:ss")
txt_age = 'Полных лет:'
txt_finalwin = 'Результаты'
txt_index = 'Индекс Руфье: '
txt_workheart = 'Работоспособность сердца: '
txt_res1 = "низкая. Срочно обратитесь к врачу!"
txt_res2 = "удовлетворительная. Обратитесь к врачу!"
txt_res3 = "средняя. Возможно, стоит дополнительно обследоваться у врача."
txt_res4 = "выше среднего"
txt_res5 = "высокая"


from PyQt5.QtCore import Qt
from PyQt5.QtWidgets import (
       QApplication, QWidget,
       QHBoxLayout, QVBoxLayout,
       QGroupBox, QRadioButton,
       QPushButton, QLabel, QListWidget, QLineEdit)
 
from instr import *
from second_win import *

class MainWin(QWidget):
   def __init__(self):
       ''' окно, в котором располагается приветствие '''
       super().__init__()
 
       # создаём и настраиваем графические элементы:
       self.initUI()
 
       #устанавливает связи между элементами
       self.connects()
 
       #устанавливает, как будет выглядеть окно (надпись, размер, место)
       self.set_appear()
 
       # старт:
       self.show()
              
   def initUI(self):
       ''' создаёт графические элементы '''
       self.btn_next = QPushButton(txt_next, self)
       self.hello_text = QLabel(txt_hello)
       self.instruction = QLabel(txt_instruction)
 
       self.layout_line = QVBoxLayout()
       self.layout_line.addWidget(self.hello_text, alignment = Qt.AlignLeft)
       self.layout_line.addWidget(self.instruction, alignment = Qt.AlignLeft)
       self.layout_line.addWidget(self.btn_next, alignment = Qt.AlignCenter)         
       self.setLayout(self.layout_line)
  
   def next_click(self):
       self.tw = TestWin()
       self.hide()
 
   def connects(self):
       self.btn_next.clicked.connect(self.next_click)
 
   ''' устанавливает, как будет выглядеть окно (надпись, размер, место) '''
   def set_appear(self):
       self.setWindowTitle(txt_title)
       self.resize(win_width, win_height)
       self.move(win_x, win_y)
 
app = QApplication([])
mw = MainWin()
app.exec_()

from PyQt5.QtCore import Qt, QTimer, QTime, QLocale
from PyQt5.QtGui import QFont, QDoubleValidator, QIntValidator
from PyQt5.QtWidgets import (
       QApplication, QWidget,
       QHBoxLayout, QVBoxLayout,
       QGroupBox, QRadioButton,
       QPushButton, QLabel, QListWidget, QLineEdit)
 
from instr import *
from final_win import *

class Experiment():
   def __init__(self, age, test1, test2, test3):
       self.age = age
       self.test1 = test1
       self.test2 = test2
       self.test3 = test3

class TestWin(QWidget):
   def __init__(self):
       ''' окно, в котором проводится опрос '''
       super().__init__()
 
       # создаём и настраиваем графические элементы:
       self.initUI()
 
       #устанавливает связи между элементами
       self.connects()
 
       #устанавливает, как будет выглядеть окно (надпись, размер, место)
       self.set_appear()
 
       # старт:
       self.show()
    
   def initUI(self):
       ''' создаёт графические элементы '''
       self.btn_next = QPushButton(txt_sendresults, self)
       self.btn_test1 = QPushButton(txt_starttest1, self)
       self.btn_test2 = QPushButton(txt_starttest2, self)
       self.btn_test3 = QPushButton(txt_starttest3, self)
       
       self.text_name = QLabel(txt_name)
       self.text_age = QLabel(txt_age)
       self.text_test1 = QLabel(txt_test1)
       self.text_test2 = QLabel(txt_test2)
       self.text_test3 = QLabel(txt_test3)
       self.text_timer = QLabel(txt_timer)
       
       self.line_name = QLineEdit(txt_hintname)
       self.line_age = QLineEdit(txt_hintage)
       self.line_test1 = QLineEdit(txt_hinttest1)
       self.line_test2 = QLineEdit(txt_hinttest2)
       self.line_test3 = QLineEdit(txt_hinttest3)
 
       self.l_line = QVBoxLayout()
       self.r_line = QVBoxLayout()
       self.h_line = QHBoxLayout()
       self.r_line.addWidget(self.text_timer, alignment = Qt.AlignCenter)
       self.l_line.addWidget(self.text_name, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.line_name, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.text_age, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.line_age, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.text_test1, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.btn_test1, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.line_test1, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.text_test2, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.btn_test2, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.text_test3, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.btn_test3, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.line_test2, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.line_test3, alignment = Qt.AlignLeft)
       self.l_line.addWidget(self.btn_next, alignment = Qt.AlignCenter)

       
       self.h_line.addLayout(self.l_line)      
       self.h_line.addLayout(self.r_line)     
       self.setLayout(self.h_line)
       
   def connects(self):
       self.btn_next.clicked.connect(self.next_click)
       self.btn_test1.clicked.connect(self.timer_test)
       self.btn_test2.clicked.connect(self.timer_sits)
       self.btn_test3.clicked.connect(self.timer_final)
       
   def timer_test(self):
       global time
       time = QTime(0,0,15)
       self.timer = QTimer()
       self.timer.timeout.connect(self.timer1Event)
       self.timer.start(1000)
   
   def timer_sits(self):
       global time
       time = QTime(0,0,30)
       self.timer = QTimer()
       self.timer.timeout.connect(self.timer2Event)
       self.timer.start(1500)
       
   def timer_final(self):
       global time
       time = QTime(0,1,0)
       self.timer = QTimer()
       self.timer.timeout.connect(self.timer3Event)
       self.timer.start(1000)
       
   def timer1Event(self):
       global time
       time = time.addSecs(-1)
       self.text_timer.setText(time.toString("hh:mm:ss"))
       self.text_timer.setFont(QFont("Times", 26, QFont.Bold))
       self.text_timer.setStyleSheet("color: rgb(0,0,0)")
       if time.toString("hh:mm:ss") == "00:00:00":
           self.timer.stop()
   
   def timer2Event(self):
       global time
       time = time.addSecs(-1)
       self.text_timer.setText(time.toString("hh:mm:ss")[6:8])
       self.text_timer.setFont(QFont("Times", 26, QFont.Bold))
       self.text_timer.setStyleSheet("color: rgb(0,0,0)")
       if time.toString("hh:mm:ss") == "00:00:00":
           self.timer.stop()
   
   def timer3Event(self):
       global time
       time = time.addSecs(-1)
       self.text_timer.setText(time.toString("hh:mm:ss"))
       self.text_timer.setFont(QFont("Times", 26, QFont.Bold))
       if int(time.toString("hh:mm:ss")[6:8]) <= 15:
           self.text_timer.setStyleSheet("color: rgb(0,255,0)")
       elif int(time.toString("hh:mm:ss")[6:8]) >= 45:
           self.text_timer.setStyleSheet("color: rgb(0,255,0)")
       else:
           self.text_timer.setStyleSheet("color: rgb(0,0,0)")
       if time.toString("hh:mm:ss") == "00:00:00":
           self.timer.stop()
   
   def set_appear(self):
       self.setWindowTitle(txt_title)
       self.resize(win_width, win_height)
       self.move(win_x, win_y)
   
   def next_click(self):
       self.hide()
       self.exp = Experiment(int(self.line_age.text()), 
                             int(self.line_test1.text()), 
                             int(self.line_test2.text()), 
                             int(self.line_test3.text()))
       self.fw = FinalWin(self.exp)        




from PyQt5.QtCore import Qt
from PyQt5.QtGui import QFont, QDoubleValidator, QIntValidator
from PyQt5.QtWidgets import (
       QApplication, QWidget,
       QHBoxLayout, QVBoxLayout,
       QGroupBox, QRadioButton,
       QPushButton, QLabel, QListWidget, QLineEdit)
 
from instr import *

class FinalWin(QWidget):
   def __init__(self, exp):
       ''' окно, в котором располагается приветствие '''
       super().__init__()
       # сохранили данные эксперимента
       self.exp = exp
       # создаём и настраиваем графические элементы
       self.initUI()
 
       #устанавливает, как будет выглядеть окно (надпись, размер, место)
       self.set_appear()
 
       # старт:
       self.show()
              
   def results(self):
       if self.exp.age < 7:
           self.index = 0
           return "Нет данных для такого возраста"
       self.index = (4 * (int(self.exp.test1) + int(self.exp.test2) + int(self.exp.test3)) - 200) / 10
       if self.exp.age == 7 or self.exp.age == 8:
           if self.index >= 21:
               return txt_res1
           elif self.index <  21 and self.index >= 17:
               return txt_res2
           elif self.index <  17 and self.index >= 12:
               return txt_res3
           elif self.index <  12 and self.index >= 6.5:
               return txt_res4
           else:
               return txt_res5
       if self.exp.age == 9 or self.exp.age == 10:
           if self.index >= 19.5:
               return txt_res1
           elif self.index < 19.5 and self.index >= 15.5:
               return txt_res2
           elif self.index < 15.5 and self.index >= 10.5:
               return txt_res3
           elif self.index < 10.5 and self.index >= 5:
               return txt_res4
           else:
               return txt_res5
       if self.exp.age == 11 or self.exp.age == 12:
           if self.index >= 18:
               return txt_res1
           elif self.index < 18 and self.index >= 14:
               return txt_res2
           elif self.index < 14 and self.index >= 9:
               return txt_res3
           elif self.index < 9 and self.index >= 3.5:
               return txt_res4
           else:
               return txt_res5
       if self.exp.age == 13 or self.exp.age == 14:
           if self.index >= 16.5:
               return txt_res1
           elif self.index < 16.5 and self.index >= 12.5:
               return txt_res2
           elif self.index < 12.5 and self.index >= 7.5:
               return txt_res3
           elif self.index < 7.5 and self.index >= 2:
               return txt_res4
           else:
               return txt_res5
       if self.exp.age >= 15:
           if self.index >= 15:
               return txt_res1
           elif self.index < 15 and self.index >= 11:
               return txt_res2
           elif self.index < 11 and self.index >= 6:
               return txt_res3
           elif self.index < 6 and self.index >= 0.5:
               return txt_res4
           else:
               return txt_res5
          
   
   def initUI(self):
       ''' создаёт графические элементы '''
       self.workh_text = QLabel(txt_workheart + str(self.results()))
       self.index_text = QLabel(txt_index + str(self.index))
 
       self.layout_line = QVBoxLayout()
       self.layout_line.addWidget(self.index_text, alignment = Qt.AlignCenter)
       self.layout_line.addWidget(self.workh_text, alignment = Qt.AlignCenter)        
       self.setLayout(self.layout_line)

   ''' устанавливает, как будет выглядеть окно (надпись, размер, место) '''
   def set_appear(self):
       self.setWindowTitle(txt_finalwin)
       self.resize(win_width, win_height)
       self.move(win_x, win_y)
