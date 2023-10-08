from PyQt5.QtCore import *
from PyQt5.QtWidgets import *
from PyQt5.QtGui import *

app = QApplication([])
window = QWidget()
window.setWindowTitle('Тест на знание стран')
window.setFont(QFont('Courier New', 12))
window.setWindowIcon(QIcon('icon_world.png'))
main_layout = QVBoxLayout()
question = QLabel('Сколько стран вы знаете?')
main_layout.addWidget(question, alignment=Qt.AlignCenter)

group_buttons = QGroupBox('Варианты ответа')
answer1 = QRadioButton('1')
answer2 = QRadioButton('2')
answer3 = QRadioButton('3')
answer4 = QRadioButton('4')

layout_btn_main = QHBoxLayout()
layout_btn_left = QVBoxLayout()
layout_btn_left.addWidget(answer1, alignment=Qt.AlignCenter)
layout_btn_left.addWidget(answer2, alignment=Qt.AlignCenter)
layout_btn_right = QVBoxLayout()
layout_btn_right.addWidget(answer3, alignment=Qt.AlignCenter)
layout_btn_right.addWidget(answer4, alignment=Qt.AlignCenter)

layout_btn_main.addLayout(layout_btn_left)
layout_btn_main.addLayout(layout_btn_right)

group_buttons.setLayout(layout_btn_main)
main_layout.addWidget(group_buttons, alignment=Qt.AlignCenter)

correct_answer_group = QGroupBox('Ответ')
line_answer = QVBoxLayout()
yes_or_no = QLabel('Верно/Неверно')
correct_answer = QLabel('Правильный ответ')
percent = QLabel('Процент ответов:')
line_answer.addWidget(yes_or_no, alignment=Qt.AlignCenter)
line_answer.addWidget(correct_answer, alignment=Qt.AlignCenter)
line_answer.addWidget(percent, alignment=Qt.AlignCenter)
correct_answer_group.setLayout(line_answer)
main_layout.addWidget(correct_answer_group)

button = QPushButton('Ответить')
main_layout.addWidget(button, alignment=Qt.AlignCenter)
window.setLayout(main_layout)

class Question():
    def __init__(self, question, right_answer, wrong1, wrong2, wrong3):
        self.question = question
        self.right_answer = right_answer
        self.wrong1 = wrong1
        self.wrong2 = wrong2
        self.wrong3 = wrong3

RadioGroup = QButtonGroup()
RadioGroup.addButton(answer1)
RadioGroup.addButton(answer2)
RadioGroup.addButton(answer3)
RadioGroup.addButton(answer4)
def show_question():
    correct_answer_group.hide()
    group_buttons.show()
    button.setText('Ответить')
def show_result():
    percent.setText('<span style="color:#0000ff">\nКоличество правильных ответов</span>' + str(window.score/window.total*100) + '%')
    correct_answer_group.show()
    group_buttons.hide()
    button.setText('Следующий вопрос')
    RadioGroup.setExclusive(False)
    answer1.setChecked(False)
    answer2.setChecked(False)
    answer3.setChecked(False)
    answer4.setChecked(False)
    RadioGroup.setExclusive(True)
def start_test():
    if button.text() == 'Ответить':
        check_answer()
    else:
        next_question()
from random import*
answers = [answer1, answer2, answer3, answer4]
def ask(q):
    shuffle(answers)
    answers[0].setText(q.right_answer)
    answers[1].setText(q.wrong1)
    answers[2].setText(q.wrong2)
    answers[3].setText(q.wrong3)
    question.setText(q.question)
    correct_answer.setText(q.right_answer)
    show_question()
def show_correct(res):
    yes_or_no.setText(res)
    show_result()
def check_answer():
    if answers[0].isChecked():
        window.score += 1
        show_correct('<span style="color:#00ff00">Правильно!</span>')
    if answers[1].isChecked() or answers[2].isChecked() or answers[3].isChecked():
        show_correct('<span style="color:#ff0000">Неправильно!</span>')
def next_question():
    window.count_question += 1
    window.total += 1
    if window.count_question == len(question_list):
        shuffle(question_list)
        window.count_question = 0
    ask(question_list[window.count_question])
button.clicked.connect(start_test)
q1 = Question('Что такое же большое, как слон, но ничего не весит?', 'Тень', 'Слон', 'Свет', 'Шар')
q2 = Question('Сколько лет Земле?', '4,54 миллиарда лет', '20-40 милионов лет', '75000', '4 млн лет')
q3 = Question('Когда погаснет Солнце?', 'через 5,5 млрд лет', 'через 1 млн лет', 'через 1 млрд лет', 'через 5 млн лет')
q4 = Question('Из чего сделаны облака?', 'из капли воды и кристаллов', 'из ваты', 'из газа', 'из кристаллов')
q5 = Question('Самым мелким морем на нашей планете является?', 'Мертвое', 'Байкал', 'Черное', 'Азовское')
q6 = Question('Земля вращается по орбите вокруг?', 'солнца', 'луны', 'костра', 'марса')
q7 = Question('Легкий ветер - это', 'шторм', 'бриз', 'цунами', 'буря')
question_list = [q1, q2, q3, q4, q5, q6, q7]
window.count_question = -1

window.total = 0
window.score = 0

next_question()
window.show()
app.exec_()
