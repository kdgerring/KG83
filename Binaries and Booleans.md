

    from IPython.html import *
    from IPython.display import *
    from IPython.utils import *
    from __future__ import print_function
    from IPython.html.widgets import interact, interactive, fixed
    from IPython.html import widgets
    import orderedmultidict
    from orderedmultidict import *
    
    import sympy
    import toolz
    from toolz import *
    from sympy import *
    from sympy.logic import *
    import operator, unicodedata
    from operator import *
    from unicodedata import *
    '''
    '__iand__': inclusive and == '⊼' == '∧'
    '__ior__': inclusive or; evaluates rel to the first owner
        ior({1, 2, 3, 4, 5}, {2, 3, 4})
    {1, 2, 3, 4, 5}
    
    
    __ior__ == '|' == A.difference(B) == logical_or = '∨'
    
    {1, 2, 3, 4, 5}.difference({2, 3, 4})
    {1, 5}
    
    
    {0, 1, 2, 3}.symmetric_difference({1})
    >>>
    {0, 2, 3}
    
    __ixor__: exclusive or:
            {1, 2, 3, 4, 5}.__ixor__({2, 3, 4})
            {1, 5} is for the first; whatever is NOT in the second one stays; all else leaves
            
            
    __ixor__ == '^'  '⊻'
    nor = '⊽'
    
    
    Misc:
    'isdisjoint', 'issubset',     
    difference = 'all elements that are in This set but NOT the others.'
    intersection = 'return whats in BOTH'
    symmetric_difference = 'all elements that are in exactly ONE of the sets.'
    union = 'all elements that are in Either set.'''




    "\n'__iand__': inclusive and == '⊼' == '∧'\n'__ior__': inclusive or; evaluates rel to the first owner\n    ior({1, 2, 3, 4, 5}, {2, 3, 4})\n{1, 2, 3, 4, 5}\n\n\n__ior__ == '|' == A.difference(B) == logical_or = '∨'\n\n{1, 2, 3, 4, 5}.difference({2, 3, 4})\n{1, 5}\n\n\n{0, 1, 2, 3}.symmetric_difference({1})\n>>>\n{0, 2, 3}\n\n__ixor__: exclusive or:\n        {1, 2, 3, 4, 5}.__ixor__({2, 3, 4})\n        {1, 5} is for the first; whatever is NOT in the second one stays; all else leaves\n        \n        \n__ixor__ == '^'  '⊻'\nnor = '⊽'\n\n\nMisc:\n'isdisjoint', 'issubset',     \ndifference = 'all elements that are in This set but NOT the others.'\nintersection = 'return whats in BOTH'\nsymmetric_difference = 'all elements that are in exactly ONE of the sets.'\nunion = 'all elements that are in Either set."




    y = ('⇒','→','⊃','⇔','≡','↔','¬','˜','!','∧','•','&','∨','+','ǀ','⊕','⊻','⊤','T','1','⊥','F','0','∀','()','∃','⊢')
    for x in y: 
        print("'{}': '{}',".format(x, name(x)))
        
        

    '⇒': 'RIGHTWARDS DOUBLE ARROW',
    '→': 'RIGHTWARDS ARROW',
    '⊃': 'SUPERSET OF',
    '⇔': 'LEFT RIGHT DOUBLE ARROW',
    '≡': 'IDENTICAL TO',
    '↔': 'LEFT RIGHT ARROW',
    '¬': 'NOT SIGN',
    '˜': 'SMALL TILDE',
    '!': 'EXCLAMATION MARK',
    '∧': 'LOGICAL AND',
    '•': 'BULLET',
    '&': 'AMPERSAND',
    '∨': 'LOGICAL OR',
    '+': 'PLUS SIGN',
    'ǀ': 'LATIN LETTER DENTAL CLICK',
    '⊕': 'CIRCLED PLUS',
    '⊻': 'XOR',
    '⊤': 'DOWN TACK',
    'T': 'LATIN CAPITAL LETTER T',
    '1': 'DIGIT ONE',
    '⊥': 'UP TACK',
    'F': 'LATIN CAPITAL LETTER F',
    '0': 'DIGIT ZERO',
    '∀': 'FOR ALL',



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-86-7e88f737dfe7> in <module>()
          1 y = ('⇒','→','⊃','⇔','≡','↔','¬','˜','!','∧','•','&','∨','+','ǀ','⊕','⊻','⊤','T','1','⊥','F','0','∀','()','∃','⊢')
          2 for x in y:
    ----> 3     print("'{}': '{}',".format(x, name(x)))
          4 
          5 


    TypeError: need a single Unicode character as parameter



    """('⇒','→','⊃')|material_implication|implies;if..then|A ⇒ B is true only in the case that either A is false or B is true, or both.|
    ('⇔','≡','↔')	|material_equivalence|iff 'the same as'|A ⇔ B is true only if both A and B are false, or both A and B are true.|
    ('¬','˜','!')	    |negation|not|The statement ¬A is true if and only if A is False.|
    ('∧','•','&')	    |logical_conjunction|and|The statement A ∧ B is True if A and B are BOTH TRUE; else it is false.|
    ('∨','+','ǀ')	    |logical_inclusive_disjunction|or|The statement A ∨ B is True if A OR B (or both) are true; if both are false, the statement is false.|
    ('⊕','⊻')	        |exclusive_disjunction|xor|The statement A ⊕ B is True when either A or B, but NOT BOTH, are true. A ⊻ B means the same.|
    ('⊤','T','1')	    |Tautology|top;verum|The statement ⊤ is unconditionally TRUE.|
    ('⊥','F','0')	    |Contradiction|bottom falsum falsity|The statement ⊥ is unconditionally FALSE.|
    ('∀','()')	    |universal_qualification|for_all,for_any,for_each|∀ x: P(x) or (x) P(x) means P(x) is True for ALL x.|
    '∃'		        |existential_qualification|there_exists|∃ x: P(x) means there is AT LEAST one x such that P(x) is True.|
    '∃!'		    |uniqueness quantification|there_exists_only_one|∃! x: P(x) means there is EXACTLY ONE x such that P(x) is True.|
    ':=','≡' ,':⇔'  |definition|is_defined_as|x := y or x ≡ y means x is defined to be another name for y; congruent; logically equivalent to.|
    '(' )		    |precedence_grouping|paren|Perform the operations inside the parentheses first.|
    '⊢'		        |Turnstile|provable|x ⊢ y means y IS-Provable-from x (in some specified formal system).|"""
    def zip_it(x, rep, seq):
        return list(zip(([x]*rep), seq))
    
    zip_it('material_implication, implies, if..then', 4, ('⇒','→','⊃'))




    [('material_implication, implies, if..then', '⇒'),
     ('material_implication, implies, if..then', '→'),
     ('material_implication, implies, if..then', '⊃')]




    list(zip((["material_implication"]*3), ('⇔','≡','↔')))




    [('material_equivalence', '⇔'),
     ('material_equivalence', '≡'),
     ('material_equivalence', '↔')]




    d = omdict((('material_implication','⇒'), ('material_implication','→'), ('material_implication','⊃')))
    d.listitems()




    [('material_implication', ['⇒', '→', '⊃'])]




    New: ['left parenthesis', 'left right double arrow', 'ampersand', 'down tack', 'left right arrow', 'up tack', 'rightwards arrow', 'identical to', 'logical and', 'there exists', 'circled plus', 'for all', 'digit zero', 'not sign', 'small tilde', 'plus sign', 'right parenthesis', 'exclamation mark', 'right tack', 'latin capital letter t', 'latin letter dental click', 'rightwards double arrow', 'digit one', 'latin capital letter f', 'xor', 'logical or', 'equals sign', 'bullet', 'superset of', 'colon']



      File "<ipython-input-59-4f282d40f922>", line 1
        New: ['left parenthesis', 'left right double arrow', 'ampersand', 'down tack', 'left right arrow', 'up tack', 'rightwards arrow', 'identical to', 'logical and', 'there exists', 'circled plus', 'for all', 'digit zero', 'not sign', 'small tilde', 'plus sign', 'right parenthesis', 'exclamation mark', 'right tack', 'latin capital letter t', 'latin letter dental click', 'rightwards double arrow', 'digit one', 'latin capital letter f', 'xor', 'logical or', 'equals sign', 'bullet', 'superset of', 'colon']
           ^
    SyntaxError: invalid syntax




    name('⊤')




    'DOWN TACK'




    import sympy, toolz, itertools
    from toolz import *
    from sympy import *
    logic = {'tautology(⊤)': 										{'TT': 'true', 	'TF': 'true', 	'FT': 'true', 	'FF': 'true'},
     'logical_disjunction(Or(x,y); p | q)': 				{'TT': 'true', 	'TF': 'true', 	'FT': 'true', 	'FF': 'false',},
     'converse_implication(p if q: x<<y; Implies(y, x))':	{'TT': 'true', 	'TF': 'true', 	'FT': 'false', 	'FF': 'true'},
     #'projection_function(p)': 								{'TT': 'true', 	'TF': 'true', 	'FT': 'false', 	'FF': 'false',},
    'material_implication(if p then q; x>>y; Implies(x, y))':{'TT': 'true', 	'TF': 'false', 	'FT': 'true', 	'FF': 'true'},
     #'projection_function(q)': 								{'TT': 'true', 	'TF': 'false', 	'FT': 'true', 	'FF': 'false',},
     'logical_biconditional(Not(Xor(x, y))Xnor:p iff q)': 	{'TT': 'true', 	'TF': 'false', 	'FT': 'false', 	'FF': 'true'},
     'logical_conjunction(p ∧ q, X&Y, And(X, Y))': 			{'TT': 'true', 	'TF': 'false', 	'FT': 'false', 	'FF': 'false',},
    
     'logical_nand(Nand(x, y);~(X&Y)p ↑ q; Not(And(x, y)))':{'TT': 'false', 'TF': 'true', 	'FT': 'true', 	'FF': 'true'},
     'exclusive_disjunction(Xor(x, y):p ⊕ q; x^y)': 				{'TT': 'false', 'TF': 'true', 	'FT': 'true', 	'FF': 'false',},
     #'negation(¬q)': 										{'TT': 'false', 'TF': 'true', 	'FT': 'false', 	'FF': 'true'},
     'material_nonimplication(p->q; ~X&Y; And(Not(X), Y))': {'TT': 'false', 'TF': 'true', 	'FT': 'false', 	'FF': 'false',},
     #'negation(~p)': 										{'TT': 'false', 'TF': 'false', 	'FT': 'true', 	'FF': 'true'},
     'converse_nonimplication(Not(Implies(x, y)), ~(x<<y))':{'TT': 'false', 'TF': 'false', 	'FT': 'true', 	'FF': 'false',},
     'logical_nor(Nor(x, y); ~(y|x))': 						{'TT': 'false', 'TF': 'false', 	'FT': 'false', 	'FF': 'true'},
    'contradiction(⊥)': 									{'TT': 'false', 'TF': 'false', 	'FT': 'false', 	'FF': 'false',}}
    truth_keys = ['contradiction(⊥)', 'negation(¬q)', 'logical_nor(NOR)', 'logical_conjunction(p ∧ q)', 'converse_nonimplication(<-p)', 'logical_nand(p ↑ q)', 'projection_function(q)', 'logical_biconditional(XNOR:p iff q)', 'exclusive_disjunction(XOR:p ⊕ q)', 'projection_function(p)', 'material_nonimplication(p->q)', 'logical_disjunction(Or:p ∨ q)', 'converse_implication(p if q: p <- q)', 'negation(~p)', 'material_implication(if p then q)', 'tautology(⊤)']
    
    def cmp(x, output=dict):
    	truth_keys = ['contradiction(⊥)', 'negation(¬q)', 'logical_nor(NOR)', 'logical_conjunction(p ∧ q)', 'converse_nonimplication(<-p)', 'logical_nand(p ↑ q)', 'projection_function(q)', 'logical_biconditional(XNOR:p iff q)', 'exclusive_disjunction(XOR:p ⊕ q)', 'projection_function(p)', 'material_nonimplication(p->q)', 'logical_disjunction(Or:p ∨ q)', 'converse_implication(p if q: p <- q)', 'negation(~p)', 'material_implication(if p then q)', 'tautology(⊤)']
    	result = {}
    	results = []
    	for t in truth_keys:
    		result[t] = Truth[t][x]
    		results.append([t, Truth[t][x]])
    	if output == 'dict':
    		return result
    	else:
    		return results
    
    def meta_sorting(x, y):
    	Truth = logic.copy()
    	truth_keys = ['contradiction(⊥)', 'negation(¬q)', 'logical_nor(NOR)', 'logical_conjunction(p ∧ q)', 'converse_nonimplication(<-p)', 'logical_nand(p ↑ q)', 'projection_function(q)', 'logical_biconditional(XNOR:p iff q)', 'exclusive_disjunction(XOR:p ⊕ q)', 'projection_function(p)', 'material_nonimplication(p->q)', 'logical_disjunction(Or:p ∨ q)', 'converse_implication(p if q: p <- q)', 'negation(~p)', 'material_implication(if p then q)', 'tautology(⊤)']
    	true = []
    	false = []
    	TT, TF, FT, FF = [], [], [], []
    	for t in truth_keys:
    		if Truth[t][x] == 'true':
    			true.append(t)
    		elif Truth[t][x]== 'false':
    			false.append(t)
    	for t1 in true:
    		if Truth[t1][y] == 'true':
    			TT.append(t1)
    		if Truth[t1][y] == 'false':
    			TF.append(t1)
    	for t1 in false:
    			if Truth[t1][y] == 'true':
    				FT.append(t1)
    			if Truth[t1][y] == 'false':
    				FF.append(t1)
    			return("{}\n{}\n{}\n{}".format(TT, TF, FT, FF))
    
    
    
    #print(meta_sorting('TT', 'FT'))
    
    
    
    
    
    def explore():
        expr_string = input("Enter an expression: ")
        expr = sympify(expr_string)
        variables = expr.free_symbols
        for truth_values in cartes([false, true], repeat=len(variables)):
            values = dict(zip(variables, truth_values))
            answer = expr.subs(values)
            _values = merge(values)
            #print('{0}: {1}'.format(list(mapcat(identity, values.items())), answer))
            print('{1}\n\t{0}'.format(values, answer))
            #for v in values.items():
                #print('{}'.format(list(zip(answer, v))))
    			#, answer))
    
    #explore('X & (Y|Z)')
    def make():
    	words = input("Enter an expression: ")
    	expr = sympify(words)
    	variables = expr.free_symbols
    	for truth_values in cartes([true, false], repeat=len(expr.free_symbols)):
    		values = list((interleave((expr.free_symbols, truth_values))))
    		ans = expr.subs(dict(zip(expr.free_symbols, truth_values)))
    		print('{} = {}'.format(values, ans))
    make()

    Enter an expression: And(Nor(X,Y))
    [Y, True, X, True] = False
    [Y, True, X, False] = False
    [Y, False, X, True] = False
    [Y, False, X, False] = True



    x,y,X,Y = symbols('x,y,X,Y')
    def evals(exp):    #http://stackoverflow.com/questions/12462747/truth-tables-in-python-using-sympy
        expr = sympify(exp)
        variables = expr.free_symbols
        for truth_tables in cartes([true, false], repeat=len(expr.free_symbols)):
            values = list((interleave((expr.free_symbols, truth_tables))))
            ans = expr.subs(dict(zip(expr.free_symbols, truth_tables)))
            print('{} = {}'.format(values, ans))
    print('Logical_Disjunction; x or y')
    evals(Or(x, y))
    print('Converse Implication;Implies(x, y);x<<y; x if y')
    evals(x<<y)
    print('material_implication; Implies(y, x); if x then y')
    evals(x>>y)
    print('logical_biconditional; Xor')
    evals(Not(Xor(x, y)))
    print('logical_conjunction')
    evals(And(x, y))
    print('logical_Nand; ~(X&Y)')
    evals(Not(And(x, y)))
    print('exclusive_disjunction;x^y')
    evals((Xor(x, y)))
    print('material_nonimplication; ~X&Y')
    evals(And(Not(x), Y))
    print('converse_nonimplication; ~(x<<y)')
    evals(Not(Implies(x, y)))
    print('logical_Nor; ~(y|x)')
    evals(Nor(x, y))

    Logical_Disjunction; x or y
    [x, True, y, True] = True
    [x, True, y, False] = True
    [x, False, y, True] = True
    [x, False, y, False] = False
    Converse Implication;Implies(x, y);x<<y; x if y
    [y, True, x, True] = True
    [y, True, x, False] = False
    [y, False, x, True] = True
    [y, False, x, False] = True
    material_implication; Implies(y, x); if x then y
    [x, True, y, True] = True
    [x, True, y, False] = False
    [x, False, y, True] = True
    [x, False, y, False] = True
    logical_biconditional; Xor
    [x, True, y, True] = True
    [x, True, y, False] = False
    [x, False, y, True] = False
    [x, False, y, False] = True
    logical_conjunction
    [x, True, y, True] = True
    [x, True, y, False] = False
    [x, False, y, True] = False
    [x, False, y, False] = False
    logical_Nand; ~(X&Y)
    [x, True, y, True] = False
    [x, True, y, False] = True
    [x, False, y, True] = True
    [x, False, y, False] = True
    exclusive_disjunction;x^y
    [x, True, y, True] = False
    [x, True, y, False] = True
    [x, False, y, True] = True
    [x, False, y, False] = False
    material_nonimplication; ~X&Y
    [Y, True, x, True] = False
    [Y, True, x, False] = True
    [Y, False, x, True] = False
    [Y, False, x, False] = False
    converse_nonimplication; ~(x<<y)
    [x, True, y, True] = False
    [x, True, y, False] = True
    [x, False, y, True] = False
    [x, False, y, False] = False
    logical_Nor; ~(y|x)
    [x, True, y, True] = False
    [x, True, y, False] = False
    [x, False, y, True] = False
    [x, False, y, False] = True



    


    


    


    
