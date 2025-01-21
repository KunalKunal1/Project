import mysql.connector

conn = mysql.connector.connect(host="localhost", user="kunal", passwd="kunal", database="library1")

def addbook():
    bn = input("Enter Book Name: ")
    c = input("Enter Book Code: ")
    t = input("Total Books: ")
    s = input("Enter Subject: ")
    data = (bn, c, t, s)
    sql = 'INSERT INTO books VALUES (%s , %s, %s, %s)'
    c = conn.cursor()
    c.execute(sql, data)
    conn.commit()

    print(">----------------------------------------------------------------------------------")
    print("Data Entered Successfully")
    main()

def issueb():
    n = input("Enter Name: ")
    r = input("Enter Registration No: ")
    co = input("Enter Book Code: ")
    d = input("Enter Date: ")
    a = "INSERT INTO issue VALUES (%s, %s, %s, %s)"
    data = (n, r, co, d)
    c = conn.cursor()
    c.execute(a, data)
    conn.commit()
    print(">----------------------------------------------------------------------------------")
    print("Book issued to: ",n)
    bookup(co, -1)

def submitb():
    n = input("Enter Name: ")
    r = input("Enter Registration No: ")
    co = input("Enter Book Code: ")
    d = input("Enter Date: ")
    a = "INSERT INTO submit VALUES (%s, %s, %s, %s)"
    data = (n, r, co, d)
    c = conn.cursor()
    c.execute(a, data)
    print(">----------------------------------------------------------------------------------")
    print("Book Submitted form: ", n)
    bookup(co, 1)

def bookup(co, u):
    a = "SELECT Total FROM books WHERE BCODE = %s"
    data = (co,)
    c = conn.cursor()
    c.execute(a, data)
    myresult = c.fetchone()
    t = myresult[0] + u
    sql = "UPDATE books SET TOTAL = %s WHERE BCODE = %s"
    d = (t, co)
    c.execute(sql, d)
    conn.commit()
    main()

def dbook():
    ac = input("Enter Book Code: ")
    a = "DELETE FROM books WHERE BCODE = %s"
    data = (ac,)
    c = conn.cursor()
    c.execute(a, data)
    conn.commit()
    main()

def dispbook():
    a = "SELECT * FROM books"
    c = conn.cursor()
    c.execute(a)
    myresult = c.fetchall()
    for i in myresult:
        print("Book Name: ", i[0])
        print("Book Code: ", i[1])
        print("Total: ", i[2])
        print(">----------------------------------------------------------------------------------")
    main()

def main():
    print("""
                                 LIBRARY MANAGER
    1. ADD BOOK
    2. ISSUE BOOK
    3. SUBMIT BOOK
    4. DELETE BOOK
    5. DISPLAY BOOK
    """)
    choice = input("Enter Task No: ")
    print(">----------------------------------------------------------------------------------")
    if choice == '1':
        addbook()
    elif choice == '2':
        issueb()
    elif choice == '3':
        submitb()
    elif choice == '4':
        dbook()
    elif choice == '5':
        dispbook()
    else:
        print("Wrong choice...............")
        main()

def pswd():
    ps = input("Enter Password: ")
    if ps == "kunal":
        main()
    else:
        print("Wrong Password")
        pswd()

pswd()
