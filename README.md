#CS2302 Lab2-B
#By: Alejandra Maciel
#Last Modified: Oct-24-2018
#Instructor: Diego Aguirre
#TA: Manoj  Pravaka  Saha
#The purpose of this code was to code 2 different solutions to make a list of passwords without repeating and keeping
#track of the number of repetitions of each password. First, using linked list and then using a dictionary. The second
#part of the Lab was to code two different sorting methods that could sort the linked list by number of repetitions;
# Bubble sort and Merge sort.


#Node class (given by professor)
class Node(object):
    password = ""
    count = -1
    next = None
    #Method to create a node with defined settings.
    def __init__(self,password,count,next):
        self.password = password
        self.count = count
        self.next = next
    #Method to get all the data from the node
    def get_data(self):
        return self.password, self.count
    #Method to set the data of the node to other specific values.
    def set_data(self, password, count):
        self.password = password
        self.count = count
#Linked List class (set the head of the list)
class Linked_List():
    head = None
    #Mewthod to create a list with an empty head
    def __init__(self):
         self.head = None
    #Method to insert a new password to the list or to increase the counter of an existent password.
    def insert(self, pswd):
        #Base case: Check if list is empty
        if self.head == None:
            self.head = Node(pswd,1,None)
            return
        #If not empty than use a temporal variable to preserve the original head pointer.
        temp = self.head
        #While the temporal variable is not empty keep comparing items with the password.
        while temp != None:
            #If the item and password match then increase the counter.
            if temp.password == pswd:
                temp.count += 1
                return
            #Else if the password doesn't match then create a new node next to the temporal.
            elif temp.next == None:
                temp.next = Node(pswd,1,None)
                return
            #Else keep changing the temporal variable to the next one in the list.
            temp = temp.next
    #Method bubble sort (Sort the linked list)
    def bubble_sort(self):
        swap = True
        #While there is some swap in the list keep running the loop
        while swap:
            #Set base variable to false
            swap = False
            #Set temporal variables to traverse the list and compare elements.
            prev = self.head
            temp = self.head.next
            #While temporal variable is not empty then keep comparing variables.
            while temp != None:
                #If the number of repetitions of the first temporal variable is smaller than the number of repetitions
                #of the second temporal variable then swap nodes and set swap variable to true.
                if prev.count < temp.count:
                    a = Node(prev.password, prev.count, prev.next)
                    prev.set_data(temp.password, temp.count)
                    temp.set_data(a.password,temp.count)
                    swap = True
                #Else keep changing the temporal variables to the next ones.
                prev = prev.next
                temp = temp.next

    #Method merge (Merge the two parts of the list into one in order)
    def merge(a, b):
        temp = None
        #If first list is empty than return the second list
        if a == None:
            return b
        #If second list is empty than return the first list
        if b == None:
            return a
        #If counter of first element on the first list is bigger than the one in th the second list then save the
        #element of the first list and  try with the the next element of the first list and the same element of the
        # second one.
        if a.count >= b.count:
            temp = a
            temp.next = merge(a.next, b)
        #Else save the first element of the second list and try with the same element of the first list and the next
        # element of the second list.
        else:
            temp = b
            temp.next = merge(a, b.next)
        #return the "saved  variable
        return temp
    #Method to divide the list in half
    def divide(head):
        #Two temporal variables to traverse the list
        slow = head
        fast = head
        #If the second variable is not empty then set to the next element
        if fast:
            fast = fast.next
            #While the second variable is not empty than keep setting the temporal variables to the next elements.
            while fast:
                fast = fast.next
                slow = slow.next
        #When the second variable is empty than set the first variable to be the middle point of the list
        mid = slow.next
        slow.next = None
        #Return the original first element and the mid element of the list
        return head, mid
    #Method merge sort(Sort the linked list)
    def merge_sort(head):
        #Base case: If the first element and the net element of the list are empty then return the nothing.
        if head == None or head.next == None:
            return None
        #Else set variables a and b to be the first element and the middle one from the division method.
        a, b = divide(head)
        #Then set the a variable to be whatever comes out of sorting itself and same with b
        a = merge_sort(a)
        b = merge_sort(b)
        #Next set the first element to be the combination/merge of both lists a and b
        head = merge(a, b)
        return head

    #Method to print the linked list
    def print_list(self):
        t = []
        temp = self.head
        while temp != None:
            t.append(temp.password + "  " + str(temp.count))
            temp = temp.next
        print(t)
#Solution A method, reads the password file and converts it to a linked list.
def solution_A(file_name):
    f = open(file_name, 'r')
    file = f.read().splitlines()

    list = Linked_List()
    #For the length of the file keep inserting the password into the list
    for i in range(len(file)):
        info = file[i].split()
        list.insert(info[1])
    #While the list is not empty then keep printing the password and number of repetitions.
    while list!= None :
        print(list.head.password + " " + str(list.head.count))
        if list.head.next == None:
            break
        list.head = list.head.next
    return list.head
#Solution B method, reads the password file and converts it to a dictionary.
def solution_B(file_name):
    f = open(file_name, 'r')
    file = f.read().splitlines()

    list =[]
    #For the length of the file keep inserting the password into the list
    for i in range(len(file)):
        info = file[i].split()
        list.append(info[1])
    #Empty dictionary variable
    pswd ={}
    #For every item in the list if the item is in the dictionary then increase its counter, else set counter to 1
    for item in list:
        if item in pswd:
            pswd[item] = pswd[item] + 1
        else:
            pswd[item] = 1
    #For every item in the dictionary print the password and count
    for item in pswd:
        print(item + " " + str(pswd[item]))
#Solution C method, reads the password file, converts it to a list and calls the bubble sort
def solution_C(file_name):
    f = open(file_name, 'r')
    file = f.read().splitlines()

    list = Linked_List()
    for i in range(len(file)):
        info = file[i].split()
        list.insert(info[1])
    # while list!= None :
    #     print(list.head.password + " " + str(list.head.count))
    #     if list.head.next == None:
    #         break
    #     list.head = list.head.next
    list.print_list()
    list.bubble_sort()
    list.print_list()




solution_A("pswd")
solution_B("pswd")
solution_C("pswd")
#Solution D is called directly as the merge sort method with the solution A (linked list) as parameter
merge_sort(solution_A("pswd"))

# bubble_sort(solution_A("pswd"))


# node = merge_sort(solution_A("pswd"))
# while node != None:
#     print(node.password + " " + str(node.count))
#     node = node.next

