# This is the practice of login things

```.py
#crypto.login.py

def simplelogin(user: str, password: str)->bool:
    '''
    :param user: string
    :param password: string
    :return: True/false if user is in database
    '''
    with open("crypto.login.csv") as file: #Get the information from csv
        database = file.readlines()
    output = False 

    for line in database:
        line_cleaned = line.strip()
        user_pass = line_cleaned.split(",") #Delete the things that don't need and split it to username and password
        if user == user_pass[0] and password == user_pass[1]:
            output = True
    return output

test1 = simplelogin("joe","123456")
print(test1)
```

```.csv
#crypto.login.csv

joe,123456
alice,password123
```
