# code
class Stack:
    def __init__(self):
        self.items = []

    def isEmpty(self):
        return self.items == []

    def push(self, item):
        self.items.append(item)

    def pop(self):
        return self.items.pop()

    def peek(self):
        return self.items[len(self.items) - 1]

    def size(self):
        return len(self.items)

    # Hàm xét độ ưu tiên của các toán tử
    def uutien(self, toantu):
        if toantu == '^':
            return 3
        elif toantu == '*' or toantu == '/':
            return 2
        elif toantu == '+' or toantu == '-':
            return 1
        else:
            return 0

    # Hàm chuyển đổi biểu thức trung tố sang hậu tố
    def trungto_hauto(self, bieuthuc):
        stack = Stack()
        hauto = ''
        i = 0
        while i < len(bieuthuc):
            if bieuthuc[i].isdigit() or bieuthuc[i] == '.':   # Xử lý số hạng và trường hợp có hai chữ số hay số thực
                so = ''
                while i < len(bieuthuc) and (bieuthuc[i].isdigit() or bieuthuc[i] == '.'):
                    so += bieuthuc[i]
                    i += 1
                hauto += so + ' '
                continue

            elif bieuthuc[i] == '(':
                stack.push('(')

            elif bieuthuc[i] == ')':
                while not stack.isEmpty() and stack.peek() != '(':
                    hauto += stack.pop() + ' '
                stack.pop()  # bỏ dấu '('

            else:  # là toán tử
                while not stack.isEmpty() and self.uutien(bieuthuc[i]) <= self.uutien(stack.peek()):
                    hauto += stack.pop() + ' '
                stack.push(bieuthuc[i])

            i += 1

        while not stack.isEmpty():
            hauto += stack.pop() + ' '

        return hauto.strip()  # bỏ khoảng trắng dư ở cuối

    # Hàm tính giá trị biểu thức hậu tố
    def tinh_gia_tri(self, bieuthuc):
        stack = Stack()
        for i in bieuthuc.split():
            try:
                stack.push(float(i))  # chuyển sang số, có thể là số thực
            except:
                a = stack.pop()
                b = stack.pop()
                if i == '+':
                    stack.push(b + a)
                elif i == '-':
                    stack.push(b - a)
                elif i == '*':
                    stack.push(b * a)
                elif i == '/':
                    stack.push(b / a)
                elif i == '^':
                    stack.push(b ** a)
        return stack.pop()


bieuthuc = input("Nhập biểu thức trung tố: ")
stack = Stack()
bieuthuc_hauto = stack.trungto_hauto(bieuthuc)
print("Biểu thức hậu tố: ", bieuthuc_hauto)
#Tính giá trị biểu thức hậu tố
ket_qua = stack.tinh_gia_tri(bieuthuc_hauto)
print("Giá trị biểu thức:", ket_qua)
