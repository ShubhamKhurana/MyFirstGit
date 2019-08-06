# LOGIN FORM BY SHUBHAM KHURANA (THIS PROGRAM CAN DEFINITELY EXPAND THINKING, I STARTED
# IT JUST TO KNOW HOW DATABASE WORKS, BUT RAN INTO THE IDEA OF TURNING IT INTO A TINY PROJECT
# I CREATED A KINDOF 2-STEP VERIFICATION PROCESS WHERE AN OTP IS SENT TO THE USER'S MAIL ID
# TO CONFIRM THE VERIFICATION OF THE USER, NO STRONG SECURITY IS DEVELOPED ON THIS PROJECT.
# THIS PROJECT WAS JUST CREATED TO LEARN TO CODE A CONCEPT, FRONTEND IS BUILT BY TKINTER LIBRARY)

import mysql.connector as sk
import tkinter as t
import smtplib
import random

connection = sk.connect(
    host = "localhost",
    user = "root",
    password = "$Ktg1999",
    database = "batch_db_new"
)

sql = connection.cursor()

def register():
    name = textbox1.get()
    city = textbox2.get()
    mobile = int(textbox3.get())
    email = textbox4.get()
    u = True

    query = "SELECT * FROM TheGreat"
    #print(query)
    sql.execute(query)
    data = sql.fetchall()

    if len(data):
        for i in data:
            if i[3] == email:
                print("This email is already registered")
                u = False
                break

    if u:
        password = textbox5.get()
        x = v1.get()
        if x == 1:
            gender = 'Male'
        elif x == 2:
            gender = 'Female'

        courses = []
        if v2.get():
            courses.append('Php')
        if v3.get():
            courses.append('Java')
        if v4.get():
            courses.append('Python')
        elif v1.get() or v2.get() or v3.get():
            courses.append('None')

        v2.set(0)
        v3.set(0)
        v4.set(0)

        if len(courses):
            courses = " ".join(courses)

        r = random.randint(1000, 9999)
        txt = str(r)

        server = smtplib.SMTP('smtp.gmail.com',587)
        server.starttls()
        server.login('skstarkenterprises007@gmail.com','weareheroes')

        server.sendmail('skstartenterprises007@gmail.com',email,txt)
        server.quit()


        enter_otp = int(input("Enter the received otp: "))

        if r == enter_otp:
            query = "INSERT INTO TheGreat VALUES('%s','%s',%d,'%s','%s','%s','%s')" \
                % (name, city, mobile, email, password, gender, courses)
            #print(query)
            sql.execute(query)
            print("Congrats, You are Registered Successfully!!!")

            connection.commit()

        else:
            print("OTP entered is incorrect!!")

root = t.Tk()
root.title("SK Form")
root.geometry("500x500")

label1 = t.Label(root, text = "Name", font = "Arial")
label1.grid(sticky = t.W, padx = 10, pady = 10, row = 0, column = 0)

label2 = t.Label(root, text = "City", font = "Arial")
label2.grid(sticky = t.W, padx = 10, pady = 10, row = 1, column = 0)

label3 = t.Label(root, text = "Mobile", font = "Arial")
label3.grid(sticky = t.W, padx = 10, pady = 10, row = 2, column = 0)

label4 = t.Label(root, text = "Email", font = "Arial")
label4.grid(sticky = t.W, padx = 10, pady = 10, row = 3, column = 0)

label5 = t.Label(root, text = "Password", font = "Arial")
label5.grid(sticky = t.W, padx = 10, pady = 10, row = 4, column = 0)

label6 = t.Label(root, text = "Gender", font = "Arial")
label6.grid(sticky = t.W, padx = 10, pady = 10, row = 5, column = 0)

label7 = t.Label(root, text = "Courses", font = "Arial")
label7.grid(sticky = t.W, padx = 10, pady = 10, row = 7, column = 0)
#--------------------------------------------------------------------------
textbox1 = t.Entry(root)
textbox1.grid(row = 0, column = 1)

textbox2 = t.Entry(root)
textbox2.grid(row = 1, column = 1)

textbox3 = t.Entry(root)
textbox3.grid(row = 2, column = 1)

textbox4 = t.Entry(root)
textbox4.grid(row = 3, column = 1)

textbox5 = t.Entry(root, show = "*")    # For password
textbox5.grid(row = 4, column = 1)
#-----------------------------------------------------------------------
v1 = t.IntVar()
r1 = t.Radiobutton(root,text = "Male",variable = v1, value = 1)
r1.grid(sticky = t.SW, row = 5, column = 1)

r2 = t.Radiobutton(root,text = "Female",variable = v1, value = 2)
r2.grid(sticky = t.W, row = 6, column = 1)

v1.set(1)
#------------------------------------------------------------------
v2 = t.IntVar()
c1 = t.Checkbutton(root, text = "Php", variable = v2)
c1.grid(sticky = t.SW,row = 7, column = 1)

v3 = t.IntVar()
c2 = t.Checkbutton(root, text = "Java", variable = v3)
c2.grid(sticky = t.W,row = 8, column = 1)

v4 = t.IntVar()
c3 = t.Checkbutton(root, text = "Python", variable = v4)
c3.grid(sticky = t.W,row = 9, column = 1)
#-------------------------------------------------------------------

button = t.Button(root, text = "Register", font = "Arial 16", command = register)
button.grid(sticky = t.W, pady = 10, row = 10, column = 1)

root.mainloop()
