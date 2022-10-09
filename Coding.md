# This is all of my codings
## the name of csv file is crypto.login.csv

```.py
import csv

def color(sentence, color): #setting color code
    codelist = ["\033[30m", '\033[31m', '\033[32m', '\033[33m', '\033[34m', '\033[35m', '\033[36m', '\033[37m']
    colorlist = ["black", "red", "green", "yellow", "blue", "magenta", "cyan", "white"]
    return (codelist[colorlist.index(color)] + sentence + "\033[0m")

def newaccount() -> bool:#Make new account system
    user = input("please enter your username you want to set")
    password = input("please enter password you want to make.")
    My_list = [[user, password]]
    f = open("crypto.login.csv", mode="a", newline="")
    writer = csv.writer(f)
    for data in My_list:
        writer.writerow(data) #put in the user and password information
    f.close()
    print("\nMaking  account success!\n")
    return [username, password]

def simplelogin(user: str, password: str) -> bool:
    with open("crypto.login.csv") as file:#get information from csv
        database = file.readlines()
    output = False

    for line in database:
        line_cleaned = line.strip()
        user_pass = line_cleaned.split(",") #let the information to the list
        if user == user_pass[0] and password == user_pass[1]:#If there is correct account, Login to it.
            output = True
    return output

def numberonly(n: str, a: int) ->int:#System that returns only number.
    a = int(a)
    if 0 < a <= 5:
        if n.isdigit():
            n = int(n)
            if a >= n >= 1:
                return n
            else:
                out = numberonly(input(color(f"Error. Please enter number 1~{a}", "red")), a)
        else:
            out = numberonly(input(color(f"Error. Please enter number 1~{a}", "red")), a)
    elif a == -100:#This is for calender. This returns that the date is exist or not.
        n = n.split("/")
        if len(n) != 3:
            out = numberonly(input(color(f"Error. Please enter the date in MM//DD//YYYY", "red")), -100)
        for i in n:
            if not i.isdigit():
                out = numberonly(input(color(f"Error. Please enter the date in MM//DD//YYYY", "red")), -100)
        if not 0 < int(n[0]) <= 12 or not 0 < int(n[1]) <= 31 or not 0 < int(n[2]) < 3000:
            out = numberonly(input(color(f"Error. Please enter the date in MM//DD//YYYY", "red")), -100)
        else:
            return "/".join(n)
    else:
        if n.isdecimal():
            n = float(n)
            return n
        else:
            out = numberonly(input(color("Error, please enter number", "red")), 0)
    return out

def home()-> int:#This is home page. This gets number 1-5 to go to next page
    print(color(f"Enter the number 1~5 that you want to use in this program\n1. See the description of this system\n2. Input your data\n3. Look at your account information and statistics\n4. Check all of your data\n5. exit", "green"))
    return numberonly(input(), 5)

def enter1():#Description
    print(color('---\nDogecoin is a cryptocurrency created by software engineers Billy Markus and Jackson Palmer, \nwho decided to create a payment system as a "joke", making fun of the wild speculation in cryptocurrencies at the time.\n It is considered both the first "meme coin", and, more specifically, the first "dog coin". \n\nAnd the tool you are using now, Dogecoin journal, is a tool to use to note down those Dogecoin transactions. \nIt can be converted into either Japanese yen or U.S. dollars, and you can check the status in an easy-to-understand manner.\n\nIt can also be used as a memo tool for other things, not just Dogecoin.', "cyan"))

def enter2():#Input information
    print(color("Please enter what you want to do\n1. Enter a transaction\n2. Withdraw a transaction\n3. Back to main menu", "green"))
    number = numberonly(input(), 4)#Get information of + or -

    if number != 3:
        if number == 1:
            Dogecoin = numberonly(input(color("Please input how much dogecoin did you buy", "blue")), 0)#Get the information
        if number == 2:
            Dogecoin = -1 * numberonly(input(color("Please input how much dogecoin did you sell", "blue")), 0)#minus it because it losing the dogecoin
        Date = numberonly(input(color("Please input the date you sell(MM/DD/YYYY)", "blue")), -100)
        RateD = numberonly(input(color("Please input the rate you sell in $Dollars$", "blue")), 0)#Get rate with ¥ ad $
        RateY = numberonly(input(color("Please input the rate you sell in ¥Yen¥", "blue")), 0)
        Memo = input(color("write memo if you have", "blue"))
        My_list = [[username, password, Dogecoin, RateD, RateY, Date, Memo]]#make a list of information
        f = open("crypto.login.csv", mode="a", newline="")
        writer = csv.writer(f)
        for data in My_list:
            writer.writerow(data)#input the data
        print("DATA INPUT SUCSESSED!!!")
        return

    else:#just back to home
        print(color("Back to home", "cyan"))
        return

def enter3(): #output inforamtion
    l = "---Start---\n"
    with open("crypto.login.csv") as file:
        database = file.readlines()
    i = 0
    time = []
    timetaken = []
    for line in database:
        line_cleaned = line.strip()
        user_pass = line_cleaned.split(",")
        if username == user_pass[0] and password == user_pass[1] and len(user_pass) >= 6:
            t = user_pass[5].split("/")#Make a list
            time += [int(t[0])*100+int(t[1])+int(t[2])*10000]
            timetaken += [i, int(t[0])*100+int(t[1])+int(t[2])*10000]#I will do with time diffrence. this will be big if time if big as well.
        i += 1
    if time == []:#You have to put the data
        l += "You have no data"
    else:
        l += "Dogecoin income $rate$          ¥rate¥          Date           Memo\n"
        totalDoge = 0
        totaldol = 0
        totalyen = 0
        for i in sorted(time):
            k = timetaken[timetaken.index(i)-1]
            inportantdata = database[k].strip().split(",")
            totalDoge += float(inportantdata[2])#Get the total amount of Dogecoin that have
            totaldol -= float(inportantdata[2])*float(inportantdata[3])
            totalyen -= float(inportantdata[2])*float(inportantdata[4])#Get the total amount of money spend and use.
            for j in range(len(inportantdata)-2):
                finaldata = inportantdata[j+2]
                length = len(finaldata)
                space = ""
                for k in range(15-length):#Let the information for a table
                    space += " "
                l += f"{finaldata} {space}"
            l += "\n"
        l += "-----------\ntotal Dogecoin  $you get$       ¥You get¥\n"
        listo = [totalDoge, totaldol, totalyen]
        for i in listo:
            space = ""
            for j in range(16-len(str(i))):
                space += " "
            l += str(i) + space
    l += "\n----end----"
    print(color(l, "blue"))


def enter4():#Let them see all of the information
    with open("crypto.login.csv") as file:
        database = file.readlines()
    output = ""

    for line in database:
        line_cleaned = line.strip()
        information = line_cleaned.split(",")
        if username == information[0] and password == information[1]:
            for i in range(2, len(information)):
                output += f"{information[i]}   "
            output += "\n"
    print(color(output, "blue"))

#From here, not def. Its just some system
print(color("Welcome to the Dogecoin simulater! ", "magenta"))
username = input(f"please enter your username. If you want to make new account, enter 'make'")#get the username or make the account
if username == "make":
    accountdata = newaccount()
    username = accountdata[0]
    password = accountdata[1]
else:
    password = input("Please enter your password")
while not simplelogin(username, password):#Do it forever until make the account or login proper
    username = input(color(f"Error. please enter your username. If you want to make new account, enter 'make'", "red"))
    if username == "make":
        print(newaccount())
    else:
        password = input(color(f"please enter your password", "red"))
print(color("\nLogin success!!\n", "cyan"))

enter = home()#Go to home
while enter != 5:#if its 5, finish. Until that, do it forever
    if enter == 1:
        enter1()
    if enter == 2:
        enter2()
    if enter == 3:
        enter3()
    if enter == 4:
        enter4()
    print(color("\n---------------------------------\n", "cyan"))
    enter = home()#Go to home
print(color("Good bye!! Have a nice day!!", "magenta"))#Fin
```
