import tkinter as tk
from tkinter import ttk
from tkinter import filedialog
from tkinter import *
import math
import os

# Инициализация основного окна
root = tk.Tk()
root.title("Моё приложение")
root.geometry("1000x800")
global Xsp, Ysp, Hsp, SPU, Xc, Yc, Hc, SPU_v_rabote,Dalnost,Ugol, Ugol_v_minut, Ugol_v_sec, time_poleta


# Создание кнопок на нижней панели
bottom_panel = ttk.Frame(root)
bottom_panel.pack(side='top')

calc_kd_btn = ttk.Button(bottom_panel, text="Расчет КД")
calc_kd_btn.pack(side='left')

time_calc_btn = ttk.Button(bottom_panel, text="Расчет Времени")
time_calc_btn.pack(side='left')

settings_btn = ttk.Button(bottom_panel, text="Настройки")
settings_btn.pack(side='left')

# Связь кнопок с формами
def open_kd_form():
    kd_form = tk.Toplevel(root)
    kd_form.title("Расчет КД")
    kd_form.geometry("800x600")  # Указываем размеры окна

    # Виджеты для формы расчета КД
    Xc = Entry(kd_form, width=10, font=("Arial", 14, "bold"))
    Xc.grid(row=0, column=3)
    label_a = Label(kd_form, text="Хц=",  font=("Arial", 14, "bold"))
    label_a.grid(row=0, column=2)
    
    Yc = Entry(kd_form, width=10, font=("Arial", 14, "bold"))
    Yc.grid(row=1, column=3)
    label_b = Label(kd_form, text="Уц=", font=("Arial", 14, "bold"))
    label_b.grid(row=1, column=2)
    
    Hc = Entry(kd_form, width=10, font=("Arial", 14, "bold"))
    Hc.grid(row=2, column=3)
    label_c = Label(kd_form, text="Нц=", font=("Arial", 14, "bold"))
    label_c.grid(row=2, column=2)
    
    SPU_v_rabote = Entry(kd_form, width=10, font=("Arial", 14, "bold"))
    SPU_v_rabote.grid(row=3, column=3)
    label_c = Label(kd_form, text="СПУ=", font=("Arial", 14, "bold"))    
    label_c.grid(row=3, column=2)
    
    Dalnost = Entry(kd_form, width=10, font=("Arial", 14, "bold"))
    Dalnost.grid(row=0, column=5)
    label_a = Label(kd_form, text="Д=",  font=("Arial", 14, "bold"))
    label_a.grid(row=0, column=4)
    
    Ugol = Entry(kd_form, width=10, font=("Arial", 14, "bold"))
    Ugol.grid(row=1, column=5)
    label_b = Label(kd_form, text="А=", font=("Arial", 14, "bold"))
    label_b.grid(row=1, column=4)
    label_c = Label(kd_form, text="\u00B0", font=("Arial", 14, "bold"))
    label_c.grid(row=1, column=6)
    
    Ugol_v_minut = Entry(kd_form, width=10, font=("Arial", 14, "bold"))
    Ugol_v_minut.grid(row=1, column=7)
    label_d = Label(kd_form, text="'", font=("Arial", 14, "bold"))
    label_d.grid(row=1, column=8)

    Ugol_v_sec = Entry(kd_form, width=10,font=("Arial", 14, "bold"))
    Ugol_v_sec.grid(row=1, column=9)
    label_e = Label(kd_form, text="''",font=("Arial", 14, "bold"))
    label_e.grid(row=1, column=10)
    
    time_poleta = Entry(kd_form, width=10,font=("Arial", 14, "bold"))
    time_poleta.grid(row=2, column=5)
    label_e = Label(kd_form, text="tпол=",font=("Arial", 14, "bold"))
    label_e.grid(row=2, column=4)
    
    rashet_KD = Button(kd_form, text='Рассчитать Дальность', command=calculate)
    rashet_KD.grid(row=8, columnspan=15)
    rashet_TIME = Button(kd_form, text='Рассчитать Время', command=Rasstoianie)
    rashet_TIME.grid(row=9, columnspan=15)
    open_button = tk.Button(kd_form, text="Открыть дополнительное окно", command=open_secondary_window)
    open_button.grid(row=4, column=3)
def open_secondary_window():
    # Создаем второе окно
    secondary_window = tk.Toplevel(root)
    secondary_window.title("Дополнительный графический интерфейс")

    # Окна ввода
    Xsp = tk.Entry(secondary_window, width=40, font=("Arial", 14, "bold"), validate="all", validatecommand=(lambda d: int(d[0]) <= 7 or "Только 7 цифр разрешено", '%S'))
    Ysp = tk.Entry(secondary_window, width=40, font=("Arial", 14, "bold"))
    Hsp = tk.Entry(secondary_window, width=40, font=("Arial", 14, "bold"))
    SPU = tk.Entry(secondary_window, width=40, font=("Arial", 14, "bold"))

    # Располагаем окна ввода
    Xsp.grid(row=3, column=0)
    Ysp.grid(row=4, column=0)
    Hsp.grid(row=5, column=0)
    SPU.grid(row=6, column=0)

    # Кнопка сохранения
    sohranit_SPU = tk.Button(secondary_window, text="Сохранить", command=save_file)
    sohranit_SPU.grid(row=0, column=0)
    # Кнопка "Загрузить данные"
    zagruzit_SPU = tk.Button(secondary_window, text="Загрузить данные", command=load_data)
    zagruzit_SPU.grid(row=1, column=0)   
def save_file():
    # Открываем диалог сохранения файла
    data1 = Xsp.get().replace('\n', '')
    data2 = Ysp.get().replace('\n', '')
    data3 = Hsp.get().replace('\n', '')
    data4 = SPU.get().replace('\n', '')
    file_path = filedialog.asksaveasfilename(initialdir="/D:\\programs\\SPY\\", title="Save File")
    if file_path:
        with open(file_path, 'w') as f:
            f.write(f"{data1}\n{data2}\n{data3}\n{data4}")
def load_data():
    """Загружает данные из текстового файла в поля ввода."""
    file_path = filedialog.askopenfilename(initialdir="/С:\\", title="Open File")
    if file_path:
        with open(file_path, 'r') as f:
            lines = f.readlines()

            # Заполнение полей ввода
            Xsp.delete(0, tk.END)
            Xsp.insert(0, lines[0])
            Ysp.delete(0, tk.END)
            Ysp.insert(0, lines[1])
            Hsp.delete(0, tk.END)
            Hsp.insert(0, lines[2])
            SPU.delete(0, tk.END)
            SPU.insert(0, lines[3])



def calculate():
    # Здесь ваш код для расчета
    # Получаем значения из входных полей
    print ("TEST")
    DeltaX = float(Xc.get()) - float(Xsp.get())
    DeltaY = float(Yc.get()) - float(Ysp.get())
    
    
    # Вычисляем расстояние и угол
    distance = math.sqrt((DeltaX**2) + (DeltaY**2))
    angle_radians = math.atan2(DeltaY, DeltaX)

    angle_degrees = math.degrees(angle_radians)
    
    # Форматируем результаты для вывода
    result_string = f'{distance:.0f}'
    deg_min_sec = format_angle(angle_degrees)
    
    # Заполняем поля выводов
    Dalnost.delete(0, tk.END)
    Dalnost.insert(0, result_string)
    Ugol.delete(0, tk.END)
    Ugol.insert(0, deg_min_sec[0])
    Ugol_v_minut.delete(0, tk.END)
    Ugol_v_minut.insert(0, deg_min_sec[1])
    Ugol_v_sec.delete(0, tk.END)
    Ugol_v_sec.insert(0, deg_min_sec[2])
    print(result_string)   
def format_angle(angle):
    """Форматирует угол в формате градусов, минут, секунд."""
    minutes = abs(int((angle - int(angle)) * 60))
    seconds = abs(round(((angle - int(angle)) * 3600) % 60, 2))
    degrees = int((angle))
    print ("угол:", degrees)
    if degrees < 0:
        degrees += 360
    return degrees, minutes, seconds
def Rasstoianie(): #РАСЧЕТ ВРЕМЕНИ ПО РАССТОЯНИЮ 
    global value1, value2, value3
    value1 = float(Dalnost.get())
    if float(value1) <= 150000:
        value2 = 5.54
    elif float(value1) <= 155000:
        value2 = 5.53
    elif float(value1) <= 160000:
        value2 = 5.52
    elif float(value1) <= 165000:
        value2 = 5.51
    elif float(value1) <= 170000:
        value2 = 5.51
    elif float(value1) <= 175000:
        value2 = 5.51
    elif float(value1) <= 180000:
        value2 =  5.50
    elif float(value1) <= 185000:
        value2 = 5.50
    elif float(value1) <= 190000:
        value2 = 5.49
    elif float(value1) <= 140000:
        value2 = 5.48
    elif float(value1) <= 250000:
        value2 = 5.49
    elif float(value1) <= 260000:
        value2 = 5.50
    elif float(value1) <= 270000:
        value2 = 5.51
    elif float(value1) <= 280000:
        value2 = 5.52
    elif float(value1) <= 290000:
        value2 = 5.53
    elif float(value1) <= 300000:
        value2 = 5.54
    elif float(value1) <= 310000:
        value2 = 5.57
    elif float(value1) <= 314000:
        value2 = 6.00
    elif float(value1) <= 330000:
        value2 = 6.03
    elif float(value1) <= 340000:
        value2 = 6.06
    elif float(value1) <= 350000:
        value2 = 6.09
    elif float(value1) <= 360000:
        value2 = 6.12
    elif float(value1) <= 370000:
        value2 = 6.15
    elif float(value1) <= 380000:
        value2 = 6.18
    elif float(value1) <= 390000:
        value2 = 6.21
    else:
        try:
            value2 = float(time_poleta.get())
        except ValueError:
            value2 = None
    # Форматируем результат для вывода
    result_string = f'{value2:.2f}'
    print(result_string)
    # Заполняем поля выводов
    time_poleta.delete(0, tk.END)
    time_poleta.insert(0, result_string)
    time_poleta.delete(0, tk.END)
    time_poleta.insert(0, value2)


def open_time_form():
    time_form = tk.Toplevel(root)
    time_form.title("Расчет Времени")
    # Ваши виджеты для формы расчета времени

def open_settings_form():
    settings_form = tk.Toplevel(root)
    settings_form.title("Настройки")
    # Ваши виджеты для формы настроек

calc_kd_btn['command'] = open_kd_form
time_calc_btn['command'] = open_secondary_window
settings_btn['command'] = open_time_form

# Основной цикл приложения
root.mainloop()
