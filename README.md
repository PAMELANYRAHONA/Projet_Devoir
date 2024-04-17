from sympy.logic.boolalg import truth_table
from sympy.logic.boolalg import Or, And
from sympy import symbols

def table_verite(fonction):
    variables = sorted(list(fonction.free_symbols))
    tt = truth_table(fonction, variables)
    print("Table de vérité de la fonction:", fonction)
    print("Variables:", variables)
    for row in tt:
        print([int(i) for i in row])

def premiere_forme_canonique(fonction):
    variables = sorted(list(fonction.free_symbols))
    minterms = []
    for row in truth_table(fonction, variables):
        if row[-1] == 1:
            minterm = []
            for i, val in enumerate(row[:-1]):
                if val:
                    minterm.append(variables[i])
                else:
                    minterm.append(~variables[i])
            minterms.append(And(*minterm))
    return Or(*minterms)

def deuxieme_forme_canonique(fonction):
    variables = sorted(list(fonction.free_symbols))
    maxterms = []
    for row in truth_table(fonction, variables):
        if row[-1] == 0:
            maxterm = []
            for i, val in enumerate(row[:-1]):
                if val:
                    maxterm.append(~variables[i])
                else:
                    maxterm.append(variables[i])
            maxterms.append(Or(*maxterm))
    return And(*maxterms)

# Exemple d'utilisation
x, y, z = symbols('x y z')
fonction = (x & y) | (~x & z)
table_verite(fonction)
print("Première forme canonique:", premiere_forme_canonique(fonction))
print("Deuxième forme canonique:", deuxieme_forme_canonique(fonction))
