from datetime import date
import pylab
from Tkinter import *
import Tkinter, Tkconstants, tkFileDialog


def main():
    print "Welcome to the advanced amortization calculator."
    print
    while(1):
        S = raw_input( "Would you like to load a loan file? (Yes/No)")
        if S == "Yes" or S == "yes":
            l = loadLoan()
            if l == None:
                print "input file has an error"
            else:
                break
        elif S == "No" or S == "no":
            while(1):
                try:
                    p = float(raw_input("Principle: "))
                except:
                    print "input invalid"
                else:
                    break
            while(1):
                try:
                    I = float(raw_input("Annualized Interest Rate: "))
                except:
                    print "input invalid"
                else:
                    break
            while(1):
                year = raw_input("Length of Loan (years) (Default=30): ")
                if year == "":
                    year = 30
                    print year
                    break
                else:
                    try:
                        year = int(year)
                    except:
                        print "input invalid"
                    else:
                        break
            while(1):
                n = raw_input("Payments Per Year (Default = 12): ")
                if n == "":
                    n = 12
                    print n
                    break
                else:
                    try:
                        n = int(n)
                    except:
                        print "input invalid"
                    else:
                        break
            while(1):
                idate = raw_input("Month of First Payment (MM/YYYY)")
                idate = idate.split("/")

                try:
                    M = int(idate[0])
                    y = int(idate[1])
                except:
                    print "input invalid"
                else:
                    if M in range(13) and M > 0:
                        break
                    else:
                        print "input invalid"
                        
            
            d = date(y,M,1)
            l = loan(p, I, year, n, d)
            break
        else:
            print "input invalid"
            l = None
    
    Menu = {1:displayTable, 2:displayGraph, 3:addExtra, 4:saveLoan, 5:printLoan}


    while(1):
        print "Main Menu:"
        print
        print "1) View Amortization Schedule"
        print "2) View Amortization Graph"
        print "3) Add Extra"
        print "4) Save Loan"
        print "5) Print Loan Information"
        print "6) Save Amortization Table to Text File"
        print "7) Save Amortization Table to CSV File"
        print "8) Exit\n"

        S = int(raw_input("Type the number of the menu item you'd like to select: "))
        if S == 8:
            return
        elif S not in Menu:
            print "Input invalid"
        else:
            Menu[S](l)

        
def displayTable(loan):
    info = loan.Amortization()
    print  "{:^10}{:^10}{:^10}{:^10}{:^12}".format("Date","Principle","Interest","Extra","Remaining")
    print "{:^10} {:^10} {:^10} {:^10} {:^12}".format("","Paid", "Paid", "Paid", "Principle")
    for i in info:  #result.append({"interest":ip, "principle_paid":pp, "extra_paid":e, "date":date.strftime("%b-%Y"), "Remaining_Principle":P})
        print "{:>10} {:>10} {:>10} {:>10} {:>12}".format( i["date"], i["principle paid"], i["interest"], i["extra paid"], i["Remaining Principle"])

def displayGraph(loan):
    info = loan.Amortization()
    dates = list()
    Remaining = list()
    Ppaid = list()
    Ipaid = list()
    temp = "nothing"
    for i in info:           
        if i["date"][4:] == temp:
            #Remaining[-1] = Remaining[-1] + i["Remaining Principle"]
            Ppaid[-1] = Ppaid[-1] + i["principle paid"]
            Ipaid[-1] = Ipaid[-1] + i["interest"]
        else:
            temp = i["date"]
            temp = temp[4:]
            dates.append(temp)
            Remaining.append(i["Remaining Principle"])
            Ppaid.append(i["principle paid"])
            Ipaid.append(i["interest"])
    fig, ax1 = pylab.subplots()
    ax2 = ax1.twinx()

    ax1.bar(dates,Ppaid, label = 'Principle Paid')
    ax1.set_title('Loan Payback')
    ax1.set_ylabel('Dollars ($)')
    ax1.set_xticklabels(dates, rotation=90)
    ax1.bar(dates,Ipaid, bottom=Ppaid, label = 'Interest Paid')
    ax1.legend()
    ax2.plot(dates,Remaining)
    pylab.show()
    return


def saveLoan(loan):
    print
    filename = raw_input("Please enter the desired filename: ")
    f = open( filename+".txt", "w")
    f.write("\n# Principle\n")
    f.write(str(loan.getPrinciple()))
    f.write("\n# Interest rate\n")
    f.write(str(loan.getInterest()))
    f.write("\n# Length of loan in years\n")
    f.write(str(loan.getYears()))
    f.write("\n# Payments per year\n")
    f.write(str(loan.getNum()))
    f.write("\n#loan start date\n")
    f.write(loan.getDate())
    f.write("\n\n#extra payments: \n")
    for i in loan.getExtras():
        f.write(i)
    f.close()
    
def printLoan(loan):
    print
    print "Principle: ", loan.getPrinciple()
    print "Interest rate: ", loan.getInterest()
    print "Length of loan in years: ", loan.getYears()
    print "Payments per year: ", loan.getNum()
    print "loan start date: ", loan.getDate()
    print("\nextra payments:")
    for i in loan.getExtras():
        print(i)
    print
def loadLoan():
    root = Tk()
    root.filename = tkFileDialog.askopenfilename()
    root.withdraw()
    root.quit()
    triggerfile = open(root.filename, "r")
    all = [ line.rstrip() for line in triggerfile.readlines() ]
    lines = []
    for line in all:
        if len(line) == 0 or line[0] == '#':
            continue
        lines.append(line)
    triggerfile.close()
    p = float(lines[0])
    i = float(lines[1])
    year = int(lines[2])
    n = int(lines[3])
    d = lines[4]
    d = d.split("/")

    try:
        M = int(d[0])
        y = int(d[1])
    except:
        print "File has invalid date"
        return None
    else:
        if M not in range(13) and M <= 0:
            print "input invalid"
            return None
            
    d = date(y,M,1)
    l = loan(p, i, year, n, d)
    printLoan(l)
    return l
    

def addExtra(loan):
    print "Extra Payment Menu"
    print
    print "1) One time Payment"
    print "2) Yearly Payment"
    print "3) Biyearly Payment"
    print "4) Monthly Payment"
    print

    menu = {1:addOnce, 2:addYearly}

    S = int(raw_input("Which type of extra payment would you like to add? "))
    loan = menu[S](loan)

    return loan


def addOnce(loan):
    while(1):
        try:
            a = float(raw_input("Enter the payment amount: "))
        except:
            print "input invalid"
        else:
            break
        
    while(1):
        idate = raw_input("Enter payment month (MM/YYYY): ")
        idate = idate.split("/")

        try:
            M = int(idate[0])
            y = int(idate[1])
        except:
            print "input invalid"
        else:
            if M in range(13) and M > 0:
                break
            else:
                print "input invalid"
    d = date(y,M,1)
    extra = oneTime(a, d)
    loan.addExtra(extra)
    return loan

def addYearly(loan):
    while(1):
        try:
            a = float(raw_input("Enter the payment amount: "))
        except:
            print "input invalid"
        else:
            break
        
    while(1):
        idate = raw_input("Enter starting payment month (MM/YYYY): ")
        idate = idate.split("/")

        try:
            M = int(idate[0])
            y = int(idate[1])
        except:
            print "input invalid"
        else:
            if M in range(13) and M > 0:
                break
            else:
                print "input invalid"
    d1 = date(y,M,1)

    while(1):
        idate = raw_input("Enter ending payment month (MM/YYYY) (default is never): ")
        if idate == "":
            M = 12
            y = 2200
            break
        idate = idate.split("/")

        try:
            M = int(idate[0])
            y = int(idate[1])
        except:
            print "input invalid"
        else:
            if M in range(13) and M > 0 and y > 1500 and y < 2200:
                break
            else:
                print "input invalid"
    d2 = date(y,M,1)

    
    extra = yearly(a, d1, d2)
    loan.addExtra(extra)
    return loan
    
class loan (object):
    def __init__ (self, principle, interest, years, numpayments, date):
        self.principle = principle
        if interest >= 1:
            self.interest = interest * .01
        else:
            self.interest = interest
        self.years = years
        self.numpayments = numpayments
        self.date = date
        self.extra = list()
        self.Amortization()
        self.endDate = self.extraEnd
          
    def Amortization(self):
        result = list()
        P = self.principle
        I = self.interest/12
        n = self.numpayments*self.years
        A = round(self.principle*(I*(1+I)**n)/((1+I)**n-1),2)
        #print A
        date = self.date
        while P > 0:
            e = 0
            for extra in self.extra:
                e = e + extra.PaymentThisMonth(date)
            ip = round(P*I,2) #interest paid this month
            if P >= A - ip: #Check to insure that we don't overpay loan
                pp = round(A - ip,2) #If there is more principle than the mortgage payment, calculate as normal
            else:
                pp = P #else, pay off the remaining principle
                e = 0 #in this case there will be no extra payment regardless of schedule
            P = round(P - pp - e,2)  #Resulting principle
            result.append({"interest":ip, "principle paid":pp, "extra paid":e, "date":date.strftime("%b-%Y"), "Remaining Principle":P})
            #print result[-1]
            m = date.month + 1
            if m == 13:
                #print date.strftime("%b-%Y")
                date = date.replace(date.year + 1, 1,1)
            else:
                date = date.replace(month = m)
        self.extraEnd = date
        return result

    def addExtra(self, extra):
        self.extra.append(extra)
        self.Amortization()

    def getExtras(self):
        for i in self.extra:
            yield str(i)

    def changePrinciple(self, principle):
        self.principle = principle
        self.Amortization()

    def changeInterest(self, interest):
        if interest >= 1:
            self.interest = interest * .01
        else:
            self.interest = interest
        self.Amortization()
    def changeYears (self, years):
        self.years = years
        self.Amortization()

    def getTimeSaved(self):
        return self.endDate - self.extraEnd
    
    def getPayoffTime(self):
        return self.extraEnd - self.date
    
    def getPrinciple(self):
        return self.principle
    
    def getInterest(self):
        return self.interest
    
    def getYears(self):
        return self.years
    
    def getNum(self):
        return self.numpayments
    
    def getDate(self):
        return self.date.strftime("%m/%Y")
        
class oneTime(object):
    def __init__ (self, amount, date):
        self.amount = amount        
        self.date = date
    
    def PaymentThisMonth(self, date):
        if self.date.strftime("%b/%Y") == date.strftime("%b/%Y"):
            return self.amount
        return 0
    def __str__(self):
        return "Once " + str(self.amount) + " " +self.date.strftime("%m/%Y")

class monthly(object):
    def __init__ (self, amount, start, end):

        self.amount = amount
        self.start = start
        self.end = end

    def PaymentThisMonth(self, date):
        if self.start <= date and self.end >= date:
            return self.amount
        return 0
    def __str__(self):
        return "Monthly " + str(self.amount) + " " +self.start.strftime("%m/%Y") + " " + self.end.strftime("%m/%Y")

class yearly(object):
    def __init__(self, amount, start, end):
        self.amount = amount
        self.start = start
        self.end = end
        
    def PaymentThisMonth(self, date):
        if self.start <= date and self.end >= date and self.start.month == date.month:
            return self.amount
        return 0

    def __str__(self):
        return "Yearly " + str(self.amount) + " " +self.start.strftime("%m/%Y") + " " + self.end.strftime("%m/%Y")

    
class biyearly(object):
    def __init__(self, amount, start, finish, month1, month2):
        self.amount = amount
        self.start = start
        self.end = finish
        self.month1 = month1
        self.month2 = month2
        
    def PaymentThisMonth(self, date):
        if self.start <= date and self.end >= date and self.month1 == date.month:
            return self.amount

        if self.start <= date and self.end >= date and self.month2 == date.month:
            return self.amount
        return 0

    def __str__(self):
        return "Monthly " + str(self.amount) + " " +self.start.strftime("%m/%Y") + " " + self.end.strftime("%m/%Y") + " " + str(self.month1) + " " + str(self.month2)


if __name__ == '__main__':
    main()
