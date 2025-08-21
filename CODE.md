# --------------------------------------
# Expression Calculator
# Infix → Postfix (RPN) → Evaluation
# --------------------------------------

class ExpressionCalculator:
    def __init__(self):
        # Operator precedence
        self.precedence = {'+': 1, '-': 1, '*': 2, '/': 2}
    
    # Check if token is operator
    def is_operator(self, c):
        return c in self.precedence
    
    # Convert Infix to Postfix using Shunting Yard algorithm
    def infix_to_postfix(self, expression):
        stack = []   # operator stack
        output = []  # postfix result
        tokens = expression.replace("(", " ( ").replace(")", " ) ").split()
        
        for token in tokens:
            if token.isnumeric():  # operands go directly to output
                output.append(token)
            elif token == '(':
                stack.append(token)
            elif token == ')':
                while stack and stack[-1] != '(':
                    output.append(stack.pop())
                stack.pop()  # remove '('
            elif self.is_operator(token):
                while (stack and stack[-1] != '(' and
                       self.precedence[stack[-1]] >= self.precedence[token]):
                    output.append(stack.pop())
                stack.append(token)
        
        # pop remaining operators
        while stack:
            output.append(stack.pop())
        
        return output
    
    # Evaluate Postfix Expression
    def evaluate_postfix(self, postfix):
        stack = []
        for token in postfix:
            if token.isnumeric():
                stack.append(int(token))
            else:
                b = stack.pop()
                a = stack.pop()
                if token == '+': stack.append(a + b)
                elif token == '-': stack.append(a - b)
                elif token == '*': stack.append(a * b)
                elif token == '/': stack.append(a / b)
        return stack[0]
    
    # Main function to calculate
    def calculate(self, expression):
        postfix = self.infix_to_postfix(expression)
        print("Postfix:", " ".join(postfix))
        return self.evaluate_postfix(postfix)


# --------------------------
# Example Usage
# --------------------------
if __name__ == "__main__":
    calc = ExpressionCalculator()
    
    # Example 1
    expr1 = "3 + 4 * ( 2 - 1 )"
    print("Infix:", expr1)
    result1 = calc.calculate(expr1)
    print("Result:", result1)
    print()
    
    # Example 2
    expr2 = "( 10 + 2 ) * 3"
    print("Infix:", expr2)
    result2 = calc.calculate(expr2)
    print("Result:", result2)
