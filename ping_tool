#修复无法重新启用进程的bug
#修复无法更换ip的bug
#修复延迟过低无法显示的bug


import os
import re
from tkinter import *
import threading
import time
import subprocess



root = Tk()
root.title('延迟检测工具')
root.geometry('450x200')
var1 = StringVar()
var2 = StringVar()
#l1_1 = Label(root,text = 'www.baidu.com',bg='white')
#l1_1.place(x = 20,y = 30)
e1 = Entry(root,bg='white',width = 28)
e1.place(x = 23,y = 30)

l1 = Label(root,textvariable = var1,bg='white',height=1,width=13)
l1.place(x = 125,y = 58)
canvas1 = Canvas(root,height=100,width=200,bg='white')
canvas1.place(x = 23,y = 80)




#l2_1 = Label(root,text = 'battle.net',height=1,width=13,bg='white')
#l2_1.place(x = 20+210,y = 30)
e2 = Entry(root,bg='white',width = 28)
e2.place(x = 20+210,y = 30)
l2 = Label(root,textvariable = var2,bg='white',height=1,width=13)
l2.place(x = 125+210,y = 58)
canvas2 = Canvas(root,height=100,width=200,bg='white')
canvas2.place(x = 20+210,y = 80)
root.update()

point1 = 0
point2 = 0

def ping1():
    global point1,p1
    ip1 = e1.get()
    print('正在变更'+ip1)
    p1 = subprocess.Popen('ping '+str(ip1)+' -t',stdout=subprocess.PIPE)
    n=1
    list=[0,0]
    a=-1
    for i in iter(p1.stdout.readline,'b'):
        i = i.decode(encoding='GBK')
        if '时间' in i:         
            pattern = re.compile(r'(?<=时间=).+?(?=ms)')
            x = re.search(pattern,i)
            print (str(ip1)+x.group(0))
            var1.set('当前延迟'+str(x.group(0))+'ms')
            num = x.group(0)                 
            
        else:
            print ('ping1请求超时')
            var1.set('请求超时')
            root.update()
            num=1001
            
        if int(num)>1000:
            num = 1000
        list.append(n)
        list.append(int(num)/10+2)
        if n<200:
            canvas1.delete(ALL)
            canvas1.create_line(list,fill='red')
            root.update()
        else:
            canvas1.delete(ALL)
            canvas1.move(canvas1.create_line(list,fill='red'),a,0)
            root.update()
        n=n+1
        if n>=200:
            del list[0:2]
            a=a-1
        if point1 == 1:
            point1 = 0
            print('正在结束循环')
            break




            
def ping2():
    global point2,p2
    ip2 = e2.get()
    print('正在变更'+ip2)
    p2 = subprocess.Popen('ping '+str(ip2)+' -t',stdout=subprocess.PIPE)
    n=1
    list=[0,0]
    a=-1
    for i in iter(p2.stdout.readline,'b'):
        i = i.decode(encoding='GBK')
        if '时间' in i:
            pattern = re.compile(r'(?<=时间=).+?(?=ms)')
            x = re.search(pattern,i)
            print (str(ip2)+x.group(0))
            var2.set('当前延迟'+str(x.group(0))+'ms')
            num = x.group(0)

        else :
            print(i)
            var2.set('请求超时')
            root.update()
            num = 1001

        if int(num)>1000:
            num = 1000
        list.append(n)
        list.append(int(num)/10+2)
        if n<200:
            canvas2.delete(ALL)
            canvas2.create_line(list,fill='green')
            root.update()
        else:
            canvas2.delete(ALL)
            canvas2.move(canvas2.create_line(list,fill='green'),a,0)
            root.update()
        n=n+1
        if n>=200:
            del list[0:2]
            a=a-1
        if point2 == 1:
            point2 = 0
            print('正在结束循环')
            break

        
point1_1 = 0
point2_1 = 0

def run1():
    global point1,point1_1,p1
    if point1_1 == 1:   #重置线程（结束当前线程，开启新线程）
        print('旧线程销毁中')
        subprocess.Popen.kill(p1)
        print('旧线程销毁完毕')
        point1 = 1
    run1 = threading.Thread(target=ping1,daemon=True)
    run1.start()
    point1_1 = 1

def run2():
    global point2,point2_1,p2
    if point2_1 == 1:
        print('旧线程销毁中')
        subprocess.Popen.kill(p2)
        print('旧线程销毁完毕')
        point2 = 1
    run2 = threading.Thread(target=ping2,daemon=True)
    run2.start()
    point2_1 = 1


b1 = Button(root, bitmap="error",text="确定", width=90, height=15, compound=LEFT,command=run1)
b1.place(x = 23,y = 57)
b2 = Button(root, bitmap="info", text="确定", width=90, height=15, compound=LEFT,command=run2)
b2.place(x = 23+210,y = 57)
root.mainloop()

