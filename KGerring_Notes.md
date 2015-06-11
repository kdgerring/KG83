
#KGerring Notes*



    import itertools, concepts, tablib, toolz, unicodedata
    from tablib import *
    from toolz import *
    from itertools import *
    from unicodedata import *
    from pprint import pprint as pprint
    import string
    from string import *


    source= ( '''
        |length|change|preference|basis    |gender   |choice |
    "O1"| one  |static|strong    |thoughts |opposite |gender |
    "O2"| one  |static|strong    |thoughts |same     |gender |
    "O3"| two  |static|none      |thoughts |opposite |gender |
    "O4"| two  |fluid |mild      |acts     |opposite |novelty|
    "O5"| two  |fluid |mild      |acts     |same     |novelty|
    "O6"|  0   |static|none      |thoughts |opposite |other  |
    "O7"|  0   |fluid |other     |thoughts |opposite |unsure |
    "O8"| zero |static|strong    |thoughts |opposite |other  |
    "O9"| NA   |fluid |other     |acts     |opposite |other  |
    ''')
    
    class Source:
        """Source Stuff"""
        def __init__(self, name, source):
            self.name=name
            self.source=source
    
    
    
        def make(name, source):
            lines=[l.partition('#')[0] for l in source.splitlines()]
            properties=[p.strip() for p in lines[1].strip('|').split('|')[1:-1]]
            objects=[l.partition('|')[0].strip() for l in lines[2:-1]]
            values=[l.partition('|')[2] for l in lines[2:-1]]
            values=[l.strip('|').split('|')[:-1] for l in values]
            for i in range(len(values)):
                values[i]=[v.strip() for v in values[i]]
            for i in range(len(values)):
                values[i]=[v.replace('0', 'False') for v in values[i]]
                values[i]=[v.replace('x', 'True') for v in values[i]]
            values=list(chain(*values))
            self.values=values
            table=list(product(obj, prop))
            tables=list(zip(table, values))
            self.rows=[t[0][0] for t in tables]
            self.columns=[t[0][1] for t in tables]
            self.cells=[t[1] for t in tables]
            self.matrix=list(zip(rows, columns, cells))
        def __repr__(self):
            return "Source:" +str(self.__dict__)
    
        def nested_set(dic, keys, value):
            for key in keys[:-1]:
                dic = dic.setdefault(key, {})
            dic[keys[-1]] = value
    
        def make_dict(table, cells):
            d={}
            for i in range(len(cells)):
                nested_set(d, table[i], cells[i])
            attr=AttrDict(d)
            return attr
    
    
    
    lines=[l.partition('#')[0] for l in source.splitlines()]
    prop=[p.strip() for p in lines[1].strip('|').split('|')[1:-1]]
    print('Properties =', prop)
    obj=[l.partition('|')[0].strip() for l in lines[2:-1]]
    print('Objects =', obj)
    values=[l.partition('|')[2] for l in lines[2:-1]]
    values=[l.strip('|').split('|')[:-1] for l in values]
    for i in range(len(values)):
        values[i]=[v.strip() for v in values[i]]
    for i in range(len(values)):
        values[i]=[v.replace('0', 'False') for v in values[i]]
        values[i]=[v.replace('x', 'True') for v in values[i]]
    value=list(chain(*values))
    print('Values =')
    pprint(values)
    table=list(product(obj, prop))
    tables=list(zip(table, value))
    rows=[t[0][0] for t in tables]
    columns=[t[0][1] for t in tables]
    cells=[t[1] for t in tables]
    
    table=list(zip(rows, columns))
    #print(table)
    
    
    def nested_set(dic, keys, value):
        for key in keys[:-1]:
            dic = dic.setdefault(key, {})
        dic[keys[-1]] = value
    d={}
    for i in range(len(cells)):
         nested_set(d, table[i], cells[i])
    pprint(d)
    
    
    SO=AttrDict(d)
    print(d.keys())
    print('SO.Asexual.basis ->',SO.Asexual.basis)


    Properties = ['length', 'change', 'preference', 'basis', 'gender']
    Objects = ['"O1"', '"O2"', '"O3"', '"O4"', '"O5"', '"O6"', '"O7"', '"O8"']
    Values =
    [['one', 'static', 'strong', 'thoughts', 'opposite'],
     ['one', 'static', 'strong', 'thoughts', 'same'],
     ['two', 'static', 'none', 'thoughts', 'opposite'],
     ['two', 'fluid', 'mild', 'acts', 'opposite'],
     ['two', 'fluid', 'mild', 'acts', 'same'],
     ['False', 'static', 'none', 'thoughts', 'opposite'],
     ['False', 'fluid', 'other', 'thoughts', 'opposite'],
     ['zero', 'static', 'strong', 'thoughts', 'opposite']]
    {'"O1"': {'basis': 'thoughts',
              'change': 'static',
              'gender': 'opposite',
              'length': 'one',
              'preference': 'strong'},
     '"O2"': {'basis': 'thoughts',
              'change': 'static',
              'gender': 'same',
              'length': 'one',
              'preference': 'strong'},
     '"O3"': {'basis': 'thoughts',
              'change': 'static',
              'gender': 'opposite',
              'length': 'two',
              'preference': 'none'},
     '"O4"': {'basis': 'acts',
              'change': 'fluid',
              'gender': 'opposite',
              'length': 'two',
              'preference': 'mild'},
     '"O5"': {'basis': 'acts',
              'change': 'fluid',
              'gender': 'same',
              'length': 'two',
              'preference': 'mild'},
     '"O6"': {'basis': 'thoughts',
              'change': 'static',
              'gender': 'opposite',
              'length': 'False',
              'preference': 'none'},
     '"O7"': {'basis': 'thoughts',
              'change': 'fluid',
              'gender': 'opposite',
              'length': 'False',
              'preference': 'other'},
     '"O8"': {'basis': 'thoughts',
              'change': 'static',
              'gender': 'opposite',
              'length': 'zero',
              'preference': 'strong'}}



    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-180-c6c4ad17f2e3> in <module>()
         91 
         92 
    ---> 93 SO=AttrDict(d)
         94 print(d.keys())
         95 print('SO.Asexual.basis ->',SO.Asexual.basis)


    NameError: name 'AttrDict' is not defined



    from IPython.display import Image
    Image(filename='/Users/Kristen/figure_1.png')




![png](output_3_0.png)




    import concepts
    from concepts import *
    import concepts, bitsets, toolz, itertools, sympy, operator
    import bitsets
    from bitsets.bases import *
    from toolz import *
    import collections
    from pprint import pprint as pp
    {'order': 0, 'symbol': '~', 'pattern':    frozenset({False, True}),'kind': 'contingency', 'index': 0}
    {'order': 1, 'symbol': '<->', 'pattern':  frozenset({(False, False), (True, True)}),                             'kind': 'equivalent', 'index': 4}
    {'order': 2, 'symbol': '>-<', 'pattern':  frozenset({(False, True), (True, False)}),                             'kind': 'complement', 'index': 6}
    {'order': 3, 'symbol': '!', 'pattern':    frozenset({(False, True), (True, False), (False, False)}),             'kind': 'incompatible', 'index': 5}
    {'order': 4, 'symbol': '->', 'pattern':   frozenset({(False, True), (False, False), (True, True)}),              'kind': 'implication', 'index': 2}
    {'order': 5, 'symbol': '<-', 'pattern':   frozenset({(True, False), (False, False), (True, True)}),               'kind': 'replication', 'index': 3}
    {'order': 6, 'symbol': 'v', 'pattern':    frozenset({(False, True), (True, False), (True, True)}),                'kind': 'subcontrary', 'index': 1}
    {'order': 7, 'symbol': '~', 'pattern':    frozenset({(False, True), (True, False), (False, False), (True, True)}), 'kind': 'orthogonal', 'index': 0}
    
    properties = [(True, True), (True, False), (False, True), (False, False)]
    obj_flags = [(['Or(X,', 'Y)', 'V', '0'], [True, True, True, False], frozenset({(False, True), (True, False), (True, True)})), (['Nand(X,Y)', '!&', '1'], [False, True, True, True], frozenset({(False, True), (True, False), (False, False)})), (['Implies(X,Y)', '-->', '3'], [True, False, True, True], frozenset({(False, True), (False, False), (True, True)})), (['Implies(Y,X)', '<--', '2'], [True, True, False, True], frozenset({(True, False), (False, False), (True, True)})), (['Not(Xor(X,Y))', '!^', '4'], [True, False, False, True], frozenset({(False, False), (True, True)})), (['Xor(X,Y)', '^', '5'], [False, True, True, False], frozenset({(False, True), (True, False)})), (['And(X,Y)', '&', '6'], [True, False, False, False], frozenset({(True, True)})), (['Not(Implies(Y,X))', '<-\\-', '7'], [False, True, False, False], frozenset({(True, False)})), (['converse_nonimplication(<-p)', '-/->', '8'], [False, False, True, False], frozenset({(False, True)})), (['logical_nor(NOR)', '!V', '9'], [False, False, False, True], frozenset({(False, False)})), (['infimum', 'F', '10'], [False, False, False, False], frozenset())]
    Rel = {'Xor(X,Y)': {'order': 'Complement', 'index': 6, 'symbol': '^', 'pattern': frozenset({(False, True), (True, False)}), 'flags': [False, True, True, False], 'kind': 'Xor(X,Y)'}, 'Or(X,Y)': {'order': 'Subcontrary', 'index': 1, 'symbol': 'V', 'pattern': frozenset({(False, True), (True, False), (True, True)}), 'flags': [True, True, True, False], 'kind': 'Or(X,Y)'}, 'And(X,Y)': {'order': '6', 'index': 7, 'symbol': '&', 'pattern': frozenset({(True, True)}), 'flags': [True, False, False, False], 'kind': 'And(X,Y)'}, 'logical_nor(NOR)': {'order': '', 'index': 10, 'symbol': '!V', 'pattern': frozenset({(False, False)}), 'flags': [False, False, False, True], 'kind': 'logical_nor(NOR)'}, 'infimum': {'order': 'Contradiction', 'index': 11, 'symbol': 'F', 'pattern': frozenset(), 'flags': [False, False, False, False], 'kind': 'infimum'}, 'converse_Nonimp(<-p)': {'order': 'Contingency', 'index': 9, 'symbol': '-/->', 'pattern': frozenset({(False, True)}), 'flags': [False, False, True, False], 'kind': 'converse_Nonimp(<-p)'}, 'Not(Implies(Y,X))': {'order': 'Equivalent', 'index': 8, 'symbol': '<-\\-', 'pattern': frozenset({(True, False)}), 'flags': [False, True, False, False], 'kind': 'Not(Implies(Y,X))'}, 'Implies(X,Y)': {'order': 'Implication', 'index': 3, 'symbol': '-->', 'pattern': frozenset({(False, True), (False, False), (True, True)}), 'flags': [True, False, True, True], 'kind': 'Implies(X,Y)'}, 'supremum': {'order': 'Tautology', 'index': 0, 'symbol': 'T', 'pattern': frozenset({(False, True), (True, False), (False, False), (True, True)}), 'flags': [True, True, True, True], 'kind': 'supremum'}, 'Implies(Y,X)': {'order': 'Replication', 'index': 4, 'symbol': '<--', 'pattern': frozenset({(True, False), (False, False), (True, True)}), 'flags': [True, True, False, True], 'kind': 'Implies(Y,X)'}, 'Nand(X,Y)': {'order': 'Incompatible', 'index': 2, 'symbol': '!&', 'pattern': frozenset({(False, True), (True, False), (False, False)}), 'flags': [False, True, True, True], 'kind': 'Nand(X,Y)'}}
    rel = {'NA': 'logical_nor(NOR)',
           'complement': 'Xor(X,Y)',
           'equivalent': 'Not(Implies(Y,X))',
           'contradiction': 'infimum',
           'implication': 'Implies(X,Y)',
           'subcontrary': 'Or(X,Y)',
           'tautology': 'supremum', '6': 'And(X,Y)',
           'replication': 'Implies(Y,X)',
           'incompatible': 'Nand(X,Y)',
           'contingency': 'converse_Nonimp(<-p)'}
    
    sexual_orientation= so= Context.fromstring('''
                  |0-1|1+  |Static|Fluid|Biased|Neutral|Thought|Act|~same|~opp|Gendered|Othered|
    Heterosexual  | x |    |  x   |     |   x  |       |   x   |   |     |   x|   x    |       |
    Homosexual    | x |    |  x   |     |   x  |       |   x   |   |    x|    |   x    |       |
    Bisexual      |   |   x|  x   |     |      |     x |   x   |   |     |    |   x    |       |
    Heteroflexible|   |   x|      |   x |   x  |       |       |  x|     |   x|   x    |       |
    Homoflexible  |   |   x|      |   x |   x  |       |       |  x|    x|    |   x    |       |
    Pan           | x |  x |      |   x |   x  |     x |   x   |   |     |    |   x    |    x  |
    InFlux        |   |   x|  x   |     |      |     x |   x   |   |     |    |        |    x  |
    Asexual       | x |   x|      |   x |   x  |       |   x   |   |     |    |   x    |       |
    Unsure_NA     | x |   x|  x   |     |      |     x |   x   |   |     |    |        |    x  |
    O0            |   |   x|      |    x|      |       |       |   |     |    |        |    x  |
     ''')
    
    Intent = so._Intent
    Extent = so._Extent
    intents = so._intents
    extents = so._extents
    Sex = so.lattice
    Relations = so.relations()
    
    
    cnt = collections.Counter([r.kind for r in Relations])
    
    
    Intent._members = ('0-1', '1+', 'Static', 'Fluid', 'Biased', 'Neutral', 'Thought', 'Act', '~same', '~opp', 'Gendered', 'Othered')
    Extent._members = ('Heterosexual', 'Homosexual', 'Bisexual', 'Heteroflexible', 'Homoflexible', 'Pan', 'InFlux', 'Asexual', 'Unsure_NA', 'O0')
    Intent_dict = dict(zip(Intent._members, extents.members()))
    Extent_dict = dict(zip(Extent._members, intents.members()))
    print(Intent_dict, Extent_dict)
    pp(Sex._map)
    
    
    
    
    
    obj=sexual_orientation.objects
    prop=sexual_orientation.properties
    bool=sexual_orientation.bools
    b=list(itertools.chain(*bool))
    
    d=[]
    
    prod=itertools.product(obj, prop)
    things=list(prod)
    for thing in things:
        ob,pro=thing[0],thing[1]
        d.append([ob,pro])
    
    
    {'Complement': frozenset({(False, True), (True, False)}),
     'Implication': frozenset({(False, True), (False, False), (True, True)}),
     'Replication': frozenset({(True, False), (False, False), (True, True)}),
     'Subcontrary': frozenset({(False, True), (True, False), (True, True)}),
     'Orthogonal': frozenset({(False, True), (True, False), (False, False), (True, True)}),
     'Equivalent': frozenset({(False, False), (True, True)}),
     'Contradiction': frozenset({False}),
     'Contingency': frozenset({False, True}),
     'Tautology': frozenset({True}),
     'Incompatible': frozenset({(False, True), (True, False), (False, False)})}
    
    
    
    uni = {'Forces': '⊩', 'SupersetOfOrEqualTo': '⊇', 'Because': '∵', 'ThereExists': '∃',
    	   'True': '⊨', 'ApproximatelyNotEqualTo': '≆', 'EqualToByDef': '≝', 'SupersetOf': '⊃',
    	   'Divides': '∣', 'Assertion': '⊦', 'Excess': '∹', 'ApproximatelyEqualTo': '≅', 'ForAll': '∀',
    	   'EmptySet': '∅', 'Between': '≬', 'Union': '∪', 'EquivalentTo': '≍', 'Intersection': '∩', 'DoesNotProve': '⊬',
    	   'SubsetOf': '⊂', 'SubsetOfOrEqualTo': '⊆', 'NOr': '⊽', 'Models': '⊧', 'ThereDoesNotExist': '∄',
    	   'Succeeds': '≻', 'OriginalOf': '⊶', 'DoesNotContainAsMember': '∌', 'ImageOf': '⊷', 'NotASupersetOf': '⊅',
    	   'NotAnElementOf': '∉', 'ElementOf': '∈', 'NotEquivalentTo': '≭', 'Precedes': '≺', 'NAnd': '⊼',
    	   'Complement': '∁', 'XOr': '⊻', 'Multiset': '⊌', 'NotASubsetOf': '⊄', 'StrictlyEquivalentTo': '≣',
    	   'ProportionalTo': '∝', 'Therefore': '∴', 'NotTrue': '⊭', 'IdenticalTo': '≡', 'ContainsAsMember': '∋',
    	   'LogicalAnd': '∧', 'LogicalOr': '∨', 'CorrespondsTo': '≘'}
    
    Template('$ForAll $ThereExists y that is an $ElementOf the Union $Union is like $Intersection X').safe_substitute(uni)

###DiGraph Concept Lattice with nodes mapped w/Fruchterman-Reingold force-directed algorithm.


    from IPython.display import Image
    Image(filename='/Users/Kristen/Lattice1.png') 
    Image(filename='/Users/Kristen/Homoflexible.png') 


    Galen = {'Substance': {'Energy': ['Heat', 'Light', 'Sound'], 'BodySubstance': ['Bile', 'Feces', 'GastricAcid', 'Milk', 'Mucus', 'Pus', 'Saliva', 'Tears', 'Urine', 'Female Ejaculate', 'Blood', 'Semen', 'Breast Milk', 'Flatus'], 'ChemicalSubstance': {}}, 'Process': {'Behavior': {'Reflex': {}, 'PatternOfBehavior': ['Avoidance', 'Practice']}, 'BodyProcess': {'ViolitionalAct': ['Toileting', 'Listening', 'Stripping', 'Tasting']}, 'MentalProcess': {'Attitude': ['Unwillingness', 'Willingness'], 'Perception': {'Exteroception': ['Hearing', 'Smell', 'Touch', 'Vision']}}}, 'Structure': {'PhysicalStructure': {'Device': {'TransportDevice': {'Tube', 'Catheter'}, 'FixationDevice': {'Pin', 'Nail', 'Screw', 'PlateRod'}, 'Other': {'SurgicalInstrument', 'Probe', 'LaboratoryMachine', 'AnimalTissue', 'ConnectingMaterial', 'CuttingTool', 'HoldingTool', 'AnimalOrgan'}, 'OpeningTool': {'Knife', 'Obturator', 'Trocar', 'SolidNeedle', 'Saw', 'Drill', 'Laser', 'Scissors'}}, 'CommunicationStructure': ['Computer', 'Paper', 'Telephone'], 'BodyPart': {'InternalBodyPart': {'NervousSystemPart': ['Brain', 'CerebralHemisphere'], 'UrinaryTractBodyPart': ['InternalUrethralOrifice', 'Ureter', 'Urethra', 'UrinaryBladder', 'UrinaryTract'], 'GITractBodyPart': ['AnalCanal', 'Colon', 'DescendingColon', 'Intestine', 'Stomach', 'Rectum', 'Colon'], 'SensoryPart': ['FundusOculi', 'InnerEar']}, 'SurfaceBodyPart': {'Genitals': {'FemaleGenitalBodyPart': ['Clitoris', 'FemaleExternalGenitalia', 'MonsPubis', 'Vulva', 'Uterus', 'Vagina'], 'GeneralGenitalSurfaceBodyPart': ['Nipple', 'Pelvis', 'Perineum', 'PubicHair', 'Thorax', 'Umbilicus'], 'MaleGenitalBodyPart': ['MaleExternalGenitalia', 'Penis', 'Scrotum', 'Testis']}, 'SurfaceOpening': ['Anus', 'ExternalAuditoryMeatus', 'Introitus', 'Mouth', 'Naris', 'UrethralMeatus'], 'Extremity': {'LongPart': ['Arm', 'Forearm', 'Leg', 'Thigh'], 'HandOrFoot': ['Foot', 'Hand'], 'JointPart': ['Ankle', 'Elbow', 'Knee', 'Wrist']}, 'HeadAndNeck': ['Neck', 'Cheek', 'Chin', 'Ear', 'ExternalAspectOfEye', 'Face', 'Forehead', 'Jaw', 'Lip', 'Nose', 'Scalp'], 'Trunk': ['Abdomen', 'Back', 'Breast', 'Chest', 'Flank', 'InguinalRegion']}}}, 'AbstractStructure': {'LogicalStructure': {'Signal': 'Speech', 'FrameOfReference': ['PlaneOfReference', 'PointOfReference', 'ReferenceStructure'], 'System': {'DigestiveSystem': (), 'FunctionalSystem': (), 'AnatomicalSystem': {'GenitoUrinarySystem': ['FemaleGenitoUrinarySystem', 'MaleGenitoUrinarySystem']}}, 'Schema': [], 'Plan': {}, 'Result': {}}, 'PsychosocialConstruct': {'PsychologicalConstruct': ['Memory', 'Faith'], 'SocialOrganisation': ()}}}, 'ModifierConcept': {'Aspect': {'Dimension': {'AbsoluteMeasurement': ['Area', 'Length', 'Level', 'Mass', 'Speed', 'Volume'], 'RelativeMeasurement': 'Quality'}, 'Status': {'OrganismStatus': {'SexStatus': ['Female', 'Male']}}, 'Morphology': ['Appearance', 'Colour', 'Shape', 'Sharpness', 'Size', 'Texture'], 'Feature': {'StructuralFeature': {'Consistency': (), 'Visibility': (), 'Position': ['AnteriorPosteriorPosition', 'LateralPosition', 'MedialLateralPosition', 'ProximalDistalPosition', 'SuperiorInferiorPosition', 'UpperLowerPosition']}, 'PhysicalFeature': ['Pressure', 'Temperature'], 'OrganismFeature': ['Age', 'Anaesthesia', 'BodyPosition', 'ClinicalSpeciality', 'GradeOfExperience', 'Gravidity', 'Sex', 'Viability'], 'SubstanceFeature': ['ChemicalFeature', 'Concentration', 'PhysicalState'], 'ProcessFeature': {'Other': ['Completeness', 'FunctionalFeature', 'ProcessActivity', 'Range', 'Scope'], 'Sensitivity': 'Resistance', 'TemporalFeature': ['Chronicity', 'Duration', 'FinishTime', 'Frequency', 'ProcessPattern', 'StartTime', 'TimeOfOccurrence'], 'ProcessActivityLevel': ['DecreasedActivityLevel', 'HighActivityLevel', 'IncreasedActivityLevel', 'LowActivityLevel'], 'VolitionalFeature': 'Urgency'}}, 'State': {'AbstractState': ['LevelState', 'PositiveNegativeState', 'QualityState', 'Quantity']}}, 'Role': {'SocialRole': ['DominantRole', 'MasterRole', 'ObjectRole', 'SlaveRole', 'SubmissiveRole', 'UserRole']}, 'Unit': {'CompositeUnit': ['AreaUnit', 'ConcentrationUnit', 'CountConcentrationUnit', 'DoseUnit', 'FlowRateUnit', 'FrequencyUnit', 'PressureUnit', 'SpeedUnit', 'VolumeUnit']}}}


    Structure = {'AbstractStructure': {'LogicalStructure': {'FrameOfReference': {'PlaneOfReference',
                                                                     'PointOfReference',
                                                                     'ReferenceStructure'},
                                                'Plan': {},
                                                'Result': {},
                                                'Schema': {},
                                                'Signal': 'Speech',
                                                'System': {'AnatomicalSystem': {'GenitoUrinarySystem': {'FemaleGenitoUrinarySystem',
                                                                                                        'MaleGenitoUrinarySystem'}},
                                                           'DigestiveSystem': (),
                                                           'FunctionalSystem': ()}},
                           'PsychosocialConstruct': {'PsychologicalConstruct': {'Faith',
                                                                                'Memory'},
                                                     'SocialOrganisation': ()}},
     'PhysicalStructure': {'BodyPart': {'InternalBodyPart': {'GITractBodyPart': {'AnalCanal',
                                                                                 'Colon',
                                                                                 'Intestine',
                                                                                 'Rectum',
                                                                                 'Stomach'},
                                                             'NervousSystemPart': {'Brain',
                                                                                   'CerebralHemisphere',
                                                                                   'SpinalCord'},
                                                             'SensoryPart': {'FundusOculi',
                                                                             'InnerEar'},
                                                             'UrinaryTractBodyPart': {'InternalUrethralOrifice',
                                                                                      'Ureter',
                                                                                      'Urethra',
                                                                                      'UrinaryBladder',
                                                                                      'UrinaryTract'}},
                                        'SurfaceBodyPart': {'Extremity': {'HandOrFoot': {'Foot',
                                                                                         'Hand'},
                                                                          'JointPart': {'Ankle',
                                                                                        'Elbow',
                                                                                        'Knee',
                                                                                        'Wrist'},
                                                                          'LongPart': {'Arm',
                                                                                       'Forearm',
                                                                                       'Leg',
                                                                                       'Thigh'}},
                                                            'Genitals': {'FemaleGenitalBodyPart': {'Clitoris',
                                                                                                   'FemaleExternalGenitalia',
                                                                                                   'MonsPubis',
                                                                                                   'Uterus',
                                                                                                   'Vagina',
                                                                                                   'Vulva'},
                                                                         'GeneralGenitalSurfaceBodyPart': {'Nipple',
                                                                                                           'Pelvis',
                                                                                                           'Perineum',
                                                                                                           'PubicHair',
                                                                                                           'Thorax',
                                                                                                           'Umbilicus'},
                                                                         'MaleGenitalBodyPart': {'MaleExternalGenitalia',
                                                                                                 'Penis',
                                                                                                 'Scrotum',
                                                                                                 'Testis'}},
                                                            'HeadAndNeck': {'Cheek',
                                                                            'Chin',
                                                                            'Ear',
                                                                            'ExternalAspectOfEye',
                                                                            'Face',
                                                                            'Forehead',
                                                                            'Jaw',
                                                                            'Lip',
                                                                            'Neck',
                                                                            'Nose',
                                                                            'Scalp'},
                                                            'SurfaceOpening': {'Anus',
                                                                               'ExternalAuditoryMeatus',
                                                                               'Introitus',
                                                                               'Mouth',
                                                                               'Naris',
                                                                               'UrethralMeatus'},
                                                            'Trunk': {'Abdomen',
                                                                      'Back',
                                                                      'Breast',
                                                                      'Chest',
                                                                      'Flank',
                                                                      'InguinalRegion'}}},
                           'CommunicationStructure': {'Computer',
                                                      'Paper',
                                                      'Telephone'},
                           'Device': {'FixationDevice': {'Nail',
                                                         'Pin',
                                                         'PlateRod',
                                                         'Screw'},
                                      'OpeningTool': {'Drill',
                                                      'Knife',
                                                      'Laser',
                                                      'Obturator',
                                                      'Saw',
                                                      'Scissors',
                                                      'SolidNeedle',
                                                      'Trocar'},
                                      'Other': {'AnimalOrgan',
                                                'AnimalTissue',
                                                'ConnectingMaterial',
                                                'CuttingTool',
                                                'HoldingTool',
                                                'LaboratoryMachine',
                                                'Probe',
                                                'SurgicalInstrument'},
                                      'TransportDevice': {'Tube', 'Catheter'}}}}



    ModifierConcept = {'Aspect': {'Dimension': {'AbsoluteMeasurement': {'Area',
                                                      'Length',
                                                      'Level',
                                                      'Mass',
                                                      'Speed',
                                                      'Volume'},
                              'RelativeMeasurement': 'Quality'},
                'Feature': {'OrganismFeature': {'Age',
                                                'Anaesthesia',
                                                'BodyPosition',
                                                'ClinicalSpeciality',
                                                'GradeOfExperience',
                                                'Gravidity',
                                                'Sex',
                                                'Viability'},
                            'PhysicalFeature': {'Temperature', 'Pressure'},
                            'ProcessFeature': {'Other': {'Completeness',
                                                         'FunctionalFeature',
                                                         'ProcessActivity',
                                                         'Range',
                                                         'Scope'},
                                               'ProcessActivityLevel': {'DecreasedActivityLevel',
                                                                        'HighActivityLevel',
                                                                        'IncreasedActivityLevel',
                                                                        'LowActivityLevel'},
                                               'Sensitivity': 'Resistance',
                                               'TemporalFeature': {'Chronicity',
                                                                   'Duration',
                                                                   'FinishTime',
                                                                   'Frequency',
                                                                   'ProcessPattern',
                                                                   'StartTime',
                                                                   'TimeOfOccurrence'},
                                               'VolitionalFeature': 'Urgency'},
                            'StructuralFeature': {'Consistency': (),
                                                  'Position': {'AnteriorPosteriorPosition',
                                                               'LateralPosition',
                                                               'MedialLateralPosition',
                                                               'ProximalDistalPosition',
                                                               'SuperiorInferiorPosition',
                                                               'UpperLowerPosition'},
                                                  'Visibility': ()},
                            'SubstanceFeature': {'ChemicalFeature',
                                                 'Concentration',
                                                 'PhysicalState'}},
                'Morphology': {'Appearance',
                               'Colour',
                               'Shape',
                               'Sharpness',
                               'Size',
                               'Texture'},
                'State': {'AbstractState': {'LevelState',
                                            'PositiveNegativeState',
                                            'QualityState',
                                            'Quantity'}},
                'Status': {'SexStatus': {'Female', 'Male'}}},
     'Role': {'SocialRole': {'DominantRole',
                             'MasterRole',
                             'ObjectRole',
                             'SlaveRole',
                             'SubmissiveRole',
                             'UserRole'}},
     'Unit': {'CompositeUnit': {'AreaUnit',
                                'ConcentrationUnit',
                                'CountConcentrationUnit',
                                'DoseUnit',
                                'FlowRateUnit',
                                'FrequencyUnit',
                                'PressureUnit',
                                'SpeedUnit',
                                'VolumeUnit'}}}



    (For original implementation, see Sebastian Banks 'Concepts' at https://github.com/xflr6/concepts)


    Process = {'Behavior': {'PatternOfBehavior': {'Practice', 'Avoidance'}, 'Reflex': {}},
     'BodyProcess': {'ViolitionalAct': {'Listening',
                                        'Stripping',
                                        'Tasting',
                                        'Toileting'}},
     'MentalProcess': {'Attitude': {'Unwillingness', 'Willingness'},
                       'Perception': {'Exteroception': {'Hearing',
                                                        'Smell',
                                                        'Touch',
                                                        'Vision'}}}}
    Substance = {'BodySubstance': {'Bile',
                       'Blood',
                       'Breast Milk',
                       'Feces',
                       'Female Ejaculate',
                       'Flatus',
                       'GastricAcid',
                       'Milk',
                       'Mucus',
                       'Pus',
                       'Saliva',
                       'Semen',
                       'Tears',
                       'Urine'},
     'Energy': {'Light', 'Sound', 'Heat'}}



    Subject = {'Mind': {'Motivations': {'Sensation': {'Pain', 'Pleasure'}, 'Profit': {'Nonmonetary', 'Children', 'Monetary'}, 'Power': {'Sub', 'Dom'}, 'Posession': {'Master', 'Slave'}, 'Context': {'Ordinary', 'Taboo'}, 'Emotion': {'Comfort', 'Shame', 'Love', 'Revenge', 'Fear', 'Humiliation', 'Bonding'}}, 'Attitude': {'Willingness', 'Unwillingness'}}, 'Body': {'Responses': {'GeneralResponses', 'SexualResponses'}, 'BodySubstance': {'GastricAcid', 'Pus', 'Semen', 'Feces', 'Tears', 'Flatus', 'Urine', 'Bile', 'BreastMilk', 'Mucus', 'Saliva', 'Blood', 'FemaleEjaculate'}, 'BodyPart': {'Abnormal', 'Correct'}}, 'Identity': {'IdentityStatus': {'Role': {'SubmissiveRole', 'OtherRole', 'SlaveRole', 'MasterRole', 'DominantRole', 'ObjectRole', 'UserRole'}, 'Self': {}, 'Anonymous': {'Actual', 'Fantasy'}}, 'IdentityPermanence': {'Fluid', 'Stable'}, 'Gender': {'Intersex', 'Alien', 'Male', 'Neutral', 'Female', 'Varying'}}}
    Object = {'Experience': {'Inexperienced': {'Novice', 'Virgin'}, 'Experienced': {'Advanced', 'Intermediate'}}, 'Gender': {'GenderPermanence': {'Fluid', 'Permanent'}, 'GenderStatus': {'Intersex', 'Alien', 'Male', 'Female', 'Neutral', 'Varying'}}, 'Number': {'Solo': {}, 'Group': {'GroupNoContact', 'GroupContact'}, 'NoNumber': 'Idea'}, 'Familiarity': {'Friendship', 'Marriage', 'Coworker', 'Affair', 'Stranger', 'Acquaintance', 'Dating'}, 'Animacy': {'Animate': {'Intoxicated', 'Unconscious', 'Restrained', 'Alert', 'Conscious', 'Deceased'}, 'Inanimate': {}, 'Species': {'Fantasy', 'Human'}}, 'Age': {'Young', 'Old'}, 'Specialty': {'Fetish', 'Ordinary'}, 'Position': {'ProximalDistalPosition', 'MedialLateralPosition', 'UpperLowerPosition', 'SuperiorInferiorPosition', 'LateralPosition', 'AnteriorPosteriorPosition'}, 'State': {'Intoxicated', 'Unconscious', 'Restrained', 'Deceased', 'Conscious', 'Alert'}}
    Event = {'Variation': ['Varied', 'NotVaried'], 'Context': ['Social', 'OtherContext'], 'Proximity': ['CloseProximity', 'FarProximity'], 'Force': {'Violence': {}, 'Manipulation': ['Politics', 'Age', 'Money', 'Fame', 'PositionInLife']}, 'Communication': {'Sound': ['Talks', 'OtherSound'], 'Visual': {'Object': {'Knows', 'Unknown'}, 'Subject': {'Self': {}, 'Other': {'OtherAware', 'OtherUnaware'}}, 'Proximity': {'CloseProximity', 'FarProximity'}}}, 'Geography': ['Location', 'Setting'], 'TemporalFeature': {'Frequency': {'Never', 'VeryFrequent', 'Infrequent', 'Frequent'}, 'Chronicity': {'Acute', 'Chronic'}, 'Duration': {'LongDuration', 'ShortDuration'}}}



    Galen['Process'] = {'Behavior': {'Reflex': {}, 'PatternOfBehavior': {'Practice', 'Avoidance'}}, 'BodyProcess': {'ViolitionalAct': {'Toileting', 'Stripping', 'Tasting', 'Listening'}}, 'MentalProcess': {'Attitude': {'Willingness', 'Unwillingness'}, 'Perception': {'Exteroception': {'Touch', 'Smell', 'Hearing', 'Vision'}}}}
    Galen['Substance'] ={'Energy': {'Light', 'Sound', 'Heat'}, 'BodySubstance': {'Pus', 'GastricAcid', 'Semen', 'Feces', 'Tears', 'Urine', 'Flatus', 'Bile', 'Mucus', 'Milk', 'Saliva', 'Blood', 'FemaleEjaculate'}}
    Galen['ModifierConcept']['Aspect']['Dimension'] = {'AbsoluteMeasurement': {'Area', 'Length', 'Level', 'Mass', 'Speed', 'Volume'}, 'RelativeMeasurement': 'Quality'}
    Galen['ModifierConcept']['Aspect']['Status'] = {'SexStatus': {'Female', 'Male'}}
    Galen['ModifierConcept']['Aspect']['Morphology'] = {'Appearance', 'Colour', 'Shape', 'Sharpness', 'Size', 'Texture'}
    Galen['ModifierConcept']['Aspect']['Feature'] = {'StructuralFeature': {'Consistency': (), 'Visibility': (), 'Position': {'AnteriorPosteriorPosition', 'LateralPosition', 'MedialLateralPosition', 'ProximalDistalPosition', 'SuperiorInferiorPosition', 'UpperLowerPosition'}}, 'PhysicalFeature': {'Pressure', 'Temperature'}, 'OrganismFeature': {'Age', 'Anaesthesia', 'BodyPosition', 'ClinicalSpeciality', 'GradeOfExperience', 'Gravidity', 'Sex', 'Viability'}, 'SubstanceFeature': {'ChemicalFeature', 'Concentration', 'PhysicalState'}, 'ProcessFeature': {'Other': {'Completeness', 'FunctionalFeature', 'ProcessActivity', 'Range', 'Scope'}, 'Sensitivity': 'Resistance', 'TemporalFeature': {'Chronicity', 'Duration', 'FinishTime', 'Frequency', 'ProcessPattern', 'StartTime', 'TimeOfOccurrence'}, 'ProcessActivityLevel': {'DecreasedActivityLevel', 'HighActivityLevel', 'IncreasedActivityLevel', 'LowActivityLevel'}, 'VolitionalFeature': 'Urgency'}}
    Galen['ModifierConcept']['Aspect']['State'] = {'AbstractState': {'LevelState', 'PositiveNegativeState', 'QualityState', 'Quantity'}}
    Galen['Structure']['AbstractStructure'] = {'LogicalStructure': {'Signal': 'Speech', 'FrameOfReference': {'PlaneOfReference', 'PointOfReference', 'ReferenceStructure'}, 'System': {'DigestiveSystem': (), 'FunctionalSystem': (), 'AnatomicalSystem': {'GenitoUrinarySystem': {'FemaleGenitoUrinarySystem', 'MaleGenitoUrinarySystem'}}}, 'Schema': {}, 'Plan': {}, 'Result': {}}, 'PsychosocialConstruct': {'PsychologicalConstruct': {'Memory', 'Faith'}, 'SocialOrganisation': ()}}
    Galen['Structure']['PhysicalStructure']['Device'] = {'TransportDevice': {'Tube', 'Catheter'}, 'FixationDevice': {'Pin', 'Nail', 'Screw', 'PlateRod'}, 'Other': {'SurgicalInstrument', 'Probe', 'LaboratoryMachine', 'AnimalTissue', 'ConnectingMaterial', 'CuttingTool', 'HoldingTool', 'AnimalOrgan'}, 'OpeningTool': {'Knife', 'Obturator', 'Trocar', 'SolidNeedle', 'Saw', 'Drill', 'Laser', 'Scissors'}} #todo: BDSM
    Galen['Structure']['PhysicalStructure']['CommunicationStructure'] = {'Computer', 'Paper', 'Telephone'}
    Galen['Structure']['PhysicalStructure']['BodyPart'] = {
    	'InternalBodyPart': {'NervousSystemPart': {'Brain', 'CerebralHemisphere', 'SpinalCord'}, 'UrinaryTractBodyPart': {'InternalUrethralOrifice', 'Ureter', 'Urethra', 'UrinaryBladder', 'UrinaryTract'}, 'GITractBodyPart': {'AnalCanal', 'Colon', 'Intestine', 'Stomach', 'Rectum', 'Colon'}, 'SensoryPart': {'FundusOculi', 'InnerEar'}},
    	'SurfaceBodyPart': {'Genitals': {'FemaleGenitalBodyPart': {'Clitoris', 'FemaleExternalGenitalia', 'MonsPubis', 'Vulva', 'Uterus', 'Vagina'}, 'GeneralGenitalSurfaceBodyPart': {'Nipple', 'Pelvis', 'Perineum', 'PubicHair', 'Thorax', 'Umbilicus'}, 'MaleGenitalBodyPart': {'MaleExternalGenitalia', 'Penis', 'Scrotum', 'Testis'}}, 'SurfaceOpening': {'Anus', 'ExternalAuditoryMeatus', 'Introitus', 'Mouth', 'Naris', 'UrethralMeatus'}, 'Extremity': {'LongPart': {'Arm', 'Forearm', 'Leg', 'Thigh'}, 'HandOrFoot': {'Foot', 'Hand'}, 'JointPart': {'Ankle', 'Elbow', 'Knee', 'Wrist'}}, 'HeadAndNeck': {'Neck', 'Cheek', 'Chin', 'Ear', 'ExternalAspectOfEye', 'Face', 'Forehead', 'Jaw', 'Lip', 'Nose', 'Scalp'}, 'Trunk': {'Abdomen', 'Back', 'Breast', 'Chest', 'Flank', 'InguinalRegion'}}}



    SubjPrep = ['With ', 'From ', 'By ']
    SubjPoss = ['my ', 'his/her ', 'their ']
    SubjObj = ['Object ', 'BodySubstance', 'BodyPart ', 'Other ']
    Subj = ['I ', 'MyPartner ', 'SomeoneElse ']
    Verb = ['give ', 'receive ', 'interact ', 'play ']
    ObjPrep= ['in ', 'on ', 'from ', 'to ', 'with ', 'towards ']
    ObjPoss = ['my ', 'their ', 'his/her ']
    ObjObj = ['BodySubstance ', 'BodyPart ', 'Other ']
    ObjKeyword = ['Theme ', 'Role ', 'Context ', 'Unique ', 'Modifier ']
    Grammar = [SubjPrep, SubjPoss, SubjObj], [Verb], [ObjPrep, ObjPoss, SubjObj, ObjKeyword]
    SubjPrep[0]+SubjPoss[0]+SubjObj[1][0]+Subj[0]+Verb[2]+ObjPrep[4]+ObjPoss[1]+ObjObj[1]




    'With my BI interact with their BodyPart '




    D = {'JJ': 'Adjective', 'OBJ': 'Sentence_Obj', 'NNPS': 'Proper_Noun_Pl', 'ADVP': 'Adverb_phrase', 'MD': 'Verb_Modal', 'LOC': 'location', 'NP': 'Noun_phrase', 'VP': 'Verb_phrase', 'NNP': 'Proper_Noun_S', 'SYM': 'symbol', 'POS': 'possessive ending', 'EX': 'existential there', 'PRP': 'purpose', 'RBS': 'Adverb_sup', '(': 'left_paren', 'PRT': 'particle', 'IN': 'Conjunction_subordinating_or_preposition', 'VBD': 'Verb_Past', ')': 'right_paren', 'WDT': 'wh-determiner', 'RB': 'Adverb', 'VBZ': 'Verb_3SingPres', '.': 'Punct_period', 'VB': 'Verb', 'EXT': 'extent', 'VBP': 'Verb_1SingPres', 'PP': 'Prepositional_phrase', 'JJS': 'Adjective_sup', 'WP$': 'wh-Pronoun_poss', 'CD': 'Number', 'PRP$': 'Pronoun_poss', 'NNS': 'Noun_Pl', 'FW': 'foreign word', 'TO': 'infinitival to', 'SBJ': 'Sentence_Subj', ',': 'Punct_comma', 'PRD': 'Predicate', 'DIR': 'direction', 'WP': 'wh-Pronoun_personal', 'SBAR': 'subordinating conjunction', 'CLR': 'closely related', 'INTJ': 'interjection', 'LS': 'list item marker', 'UH': 'interjection', 'ADJP': 'Adjective_phrase', 'CC': 'Conjunction_coordinating', 'TMP': 'temporal', 'JJR': 'Adjective_comp', 'VBN': 'Verb_PastParticiple', ':': 'Punct_Colon', 'WRB': 'wh-Adverb', 'NN': 'Noun_S', 'DT': 'determiner', 'VBG': 'Verb_gerund_PresentParticiple', 'PDT': 'predeterminer', 'RP': 'Adverb_particle', 'RBR': 'Adverb_comp'}


    stopwords = {'too', 'yours', 'has', 'only', 'him', 'me', 'each', 'same', 'more', 'hers', 'his', 'to', 'nor', 'off', 'and', 't', 'which', 'from', 'are', 'between', 'through', 'this', 'themselves', 'doing', 'a', 'our', 'all', 'just', 'on', 'what', 'these', 'there', 'where', 'until', 'do', 'its', 'in', 'we', 'now', 'your', 'both', 'how', 'for', 'of', 'so', 'will', 'after', 'while', 'theirs', 'had', 'can', 'their', 'don', 'that', 'into', 'am', 'being', 'with', 'himself', 'over', 'ourselves', 'as', 'very', 'some', 's', 'under', 'she', 'itself', 'i', 'yourself', 'yourselves', 'her', 'here', 'own', 'by', 'below', 'because', 'be', 'it', 'no', 'ours', 'if', 'but', 'out', 'any', 'those', 'not', 'at', 'most', 'who', 'did', 'about', 'above', 'myself', 'an', 'such', 'further', 'you', 'or', 'herself', 'is', 'having', 'up', 'whom', 'he', 'does', 'should', 'during', 'why', 'against', 'when', 'the', 'my', 'than', 'them', 'once', 'before', 'was', 'few', 'then', 'been', 'again', 'other', 'were', 'down', 'they', 'have'} 


    import textblob
    verb = 'receive being am is was been'
    textblob.en.tag(verb)
    ['/'.join(t) for t in textblob.en.tag(verb)]





    ['receive/VB', 'being/VBG', 'am/VBP', 'is/VBZ', 'was/VBD', 'been/VBN']




    textblob.en.parse(verb)




    'receive/VB/B-VP/O being/VBG/I-VP/O am/VBP/I-VP/O is/VBZ/I-VP/O was/VBD/I-VP/O been/VBN/I-VP/O'




    list(textblob.en.taggers.word_tokenize(verb))




    ['receive', 'being', 'am', 'is', 'was', 'been']




    [textblob.en.parse(t) for t in textblob.en.taggers.word_tokenize(verb)]





    ['receive/VB/B-VP/O',
     'being/VBG/B-VP/O',
     'am/VBP/B-VP/O',
     'is/VBZ/B-VP/O',
     'was/VBD/B-VP/O',
     'been/VBN/B-VP/O']




    def tag_it():
        seq = input('Type in a sentence')
        import textblob.en
        words_list= list(textblob.en.taggers.word_tokenize(seq))
        filtered_words = [t for t in textblob.en.taggers.word_tokenize(seq) if t not in stopwords]
        root = [textblob.en.parse(f) for f in filtered_words]
        one = [textblob.en.parse(f).split('/')[0] for f in filtered_words]
        two = [textblob.en.parse(f).split('/')[1] for f in filtered_words]
        trans = [D[textblob.en.parse(f).split('/')[1]] for f in filtered_words]
        simple = [textblob.en.parse(f).split('/')[:2] for f in filtered_words]
        print(words_list)
        print('simple', simple)
        print(trans)
       # print(root)


    Titles = [[('Body Fluids', 'Blood', 'Touching', 'blood play'), ('Body Fluids', 'Blood', 'Seeing', 'blood  cut  knife'), ('Body Fluids', 'Blood', 'Tasting', 'blood  fluid  partner  risk '), ('Body Fluids', 'Blood', 'Receiving', 'blood  risk  inside'), ('Body Fluids', 'Blood', ' menses', 'Cunnalingus lick period  fetish'), ('Body Fluids', 'Blood', ' menses', 'Intercourse period'), ('Body Fluids', 'Breast milk', 'Sucking_drinking', 'milk  drink'), ('Body Fluids', 'Breast milk', 'see_observe', 'lactate'), ('Body Fluids', 'Diapers', 'wearing', 'scat  urine'), ('Body Fluids', 'Female ejaculation', 'Giving', 'make squirt'), ('Body Fluids', 'Female Ejaculation', 'drink', 'squirt'), ('Body Fluids', 'Gas', 'Farting', 'fart(smell  do)'), ('Body Fluids', 'Human Toilet', 'Giving', 'scat  pee  role'), ('Body Fluids', 'saliva', 'Spitting(in mouth)', 'mouth'), ('Body Fluids', 'saliva', 'Spitting(on body)', 'body  dom'), ('Body Fluids', 'Scat', 'Defecate in diaper', 'diaper  ABDL'), ('Body Fluids', 'Scat', 'Animal on face', 'animal  non-human  scat  receive'), ('Body Fluids', 'Scat', 'play', 'scat'), ('Body Fluids', 'scat', 'Giving', 'scat  pooping'), ('Body Fluids', 'Scat', 'defecating in underpants', 'panties  poop'), ('Body Fluids', 'scat', 'Human toilet', ''), ('Body Fluids', 'Scat', 'swallowing', 'eat  scat'), ('Body Fluids', 'Semen', 'Swallowing', 'swallow  BJ'), ('Body Fluids', 'Semen', ' Bukkake', 'Receiving group  bukkake'), ('Body Fluids', 'Semen', ' creampie', 'Giving creampie  vaginal  sex'), ('Body Fluids', 'tears', ' crying', 'watching cry  tears'), ('Body Fluids', 'Toy', 'Litter Box', 'scat  urine  role  non-human'), ('Body Fluids', 'Toy', ' pot', 'Chamber Pot Use WS  scat  dom  power'), ('Body Fluids', 'Toys', ' catheter', 'Inserting sound  medical'), ('Body Fluids', 'Unusual scents', 'Smelling', 'smell  scent'), ('Body Fluids', 'Urine', 'licking', 'cunnalingus  urine'), ('Body Fluids', 'Urine', 'sucking', 'BJ  urine'), ('Body Fluids', 'Urine', 'urinating on body(below neck)', ''), ('Body Fluids', 'Urine', 'urinating(on face_in mouth)', 'pee  mouth  give'), ('Body Fluids', 'Urine', 'Wetting underpants or clothes', 'urine  panties'), ('Body Fluids', 'Urine', 'Human toilet', 'role  toilet  non-human'), ('Body Fluids', 'Urine', 'drinking', 'urine  drink'), ('Body Fluids', 'Urine', 'Urinate in diaper', '')], [('Body Parts', 'Breast', 'Breast_Chest Fucking', 'titty fuck  breast  sex  frot'), ('Body Parts', 'Foot', 'View', 'foot  watch'), ('Body Parts', 'Foot', 'Touch', ' during intercourse foot  fetish  sex'), ('Body Parts', 'Foot', 'Touch', ' not intercourse foot  standalone'), ('Body Parts', 'Foot', 'Toe Sucking', 'toe  foot'), ('Body Parts', 'Genitalia', ' Female', 'touch labia'), ('Body Parts', 'Genitalia', ' Female', 'Pussy pumping toy  pump'), ('Body Parts', 'Genitalia', ' Female', 'Worship slave  serve  femdom  pussy'), ('Body Parts', 'Genitals', 'Urethral Play', 'sounding  catheter'), ('Body Parts', 'Hair', 'body hair', 'bush'), ('Body Parts', 'Hair', 'Shaved Genitals(having)', 'repeat  non genita('), ('Body Parts', 'Hair', 'Shaved Genitals(on partner)', 'hair  shave  partner'), ('Body Parts', 'hair', 'Shaving', ' non-genitals shave  hair'), ('Body Parts', 'hand', 'Hand Licking', 'lick  hand'), ('Body Parts', 'Hands', 'Giving', 'handjob  hand'), ('Body Parts', 'modify', 'Anthropomorphic(self contrived or false amputation)', 'extreme  amputate'), ('Body Parts', 'Modify', ' extreme', 'Plastic Surgery plastic surgery  watch'), ('Body Parts', 'Mouth', 'tongues', 'tongue'), ('Body Parts', 'Nipples', 'Touch', ' oral nipple  breast  lick'), ('Body Parts', 'Nipples', 'Touch', ' manual nipple  breast  rub nipple'), ('Body Parts', 'Other', 'armpit hair', 'hair  armpit  natural'), ('Body Parts', 'Other', 'Finger Sucking', 'finger  suck'), ('Body Parts', 'Penis', 'Cock Worship', 'penis  fetish'), ('Body Parts', 'Tattoo', ' temporary', 'Receiving modify  tattoo  body'), ('Body Parts', 'urethra', 'urethral play', 'urethra  sounding  medical  catheter')], [('Butt Stuff', 'Intercourse', ' permitted', 'gentle  on partner anal  normal'), ('Butt Stuff', 'Intercourse', ' forced', 'forced  on partner anal  force'), ('Butt Stuff', 'Other', ' pain', 'on self ass torture  butt'), ('Butt Stuff', 'Other', ' Pain', 'on partner ass torture  butt'), ('Butt Stuff', 'Part', ' finger_self', 'penetrate  on self finger  butt  masterbate'), ('Butt Stuff', 'Part', ' finger_partner', 'penetrate partner finger  butt  partner'), ('Butt Stuff', 'Part', ' hand', 'Fisting(double) DP  hand'), ('Butt Stuff', 'Part', ' hand', 'penetrate  on self fisting  ass  self'), ('Butt Stuff', 'Part', ' hand', 'penetrate  on partner finger  butt  partner'), ('Butt Stuff', 'Part', ' penis', 'Giving anal  butt stuff  partner'), ('Butt Stuff', 'Part', ' penis', 'Receiving anal sex  fuck  partner  risk'), ('Butt Stuff', 'Part', ' prostate', 'Other prostate  butt  femdom'), ('Butt Stuff', 'Part', ' tongue', 'Rimming(anus licking) rim  butt  tongue'), ('Butt Stuff', 'Other', ' double penetration', 'double penetration DP  toy'), ('Butt Stuff', 'Other', ' lick own semen', 'licking own semen from anus of partner lick  ass  semen  body_fluid  butt_stuff  taste'), ('Butt Stuff', 'Other', ' lick other semen', "licking others' semen from anus of partner butt stuff  body fluids  semen  lick  cuck  others  risk"), ('Butt Stuff', 'Other', ' forced self toy', 'forced  on self forced  dir  butt  toy'), ('Butt Stuff', 'Other', ' forced partner toy', 'forced  on partner forced  butt  toy'), ('Butt Stuff', 'Toy', ' dildo', 'Penetrate strapon  pegging  femdom'), ('Butt Stuff', 'Toy', ' dildo', 'Suck suck  strapon  femdom'), ('Butt Stuff', 'Toy', ' dildo', 'Give strapon  femdom  anal'), ('Butt Stuff', 'Toy', ' enema', 'Giving toy  enema'), ('Butt Stuff', 'Toy', ' enema', 'Receiving toy  enema'), ('Butt Stuff', 'Toy', ' self ice enema', 'Give toy  enema  cold  ice'), ('Butt Stuff', 'Toy', ' other ice enema', 'Ice Enemas toy  enema  cold  ice'), ('Butt Stuff', 'Toy', ' ginger', 'figging toy  pain'), ('Butt Stuff', 'Toy', ' medical', 'Speculums(anal) medical  butt  toy'), ('Butt Stuff', 'Toy', ' self beads', 'Receiving beads butt toys'), ('Butt Stuff', 'Toy', ' part beads', 'Giving beads  anal  toy'), ('Butt Stuff', 'Toy', ' other beads', 'Receive beads  anal  toy'), ('Butt Stuff', 'Toy', ' self dildo', 'Inserting toys  dildo dildo partner'), ('Butt Stuff', 'Toy', ' partner dildo', 'Wear toy  dildo'), ('Butt Stuff', 'Toy', ' partner plug', 'Inserting plug  toy'), ('Butt Stuff', 'Toy', ' partner strap-on', 'Giving strap_on  femdom  role'), ('Butt Stuff', 'Toy', ' self strap-on', 'Receiving ')], [('D_S', 'Forced Exposure', 'Public Exposure', 'humiliate  public')], [('Groups', 'anonymous', 'strangers', 'group  orgy  anon'), ('Groups', 'Gentle', 'Flirting', 'gentle  talk  group'), ('Groups', 'Orgy', 'Gangbangs', 'force  group  role'), ('Groups', 'Orgy', 'Glory Hole', 'anon  orgy  cover'), ('Groups', 'Orgy', 'Orgy', ' general orgy'), ('Groups', 'orgy', 'Swapping(hard)', 'swap  group'), ('Groups', 'orgy', 'Triple Penetration(multiple partners)', 'orgy  triple  TP'), ('Groups', 'swing', 'Swapping(soft)', 'swap  group'), ('Groups', 'swing', 'Swinging', 'swing'), ('Groups', 'threesome', 'threesomes', '3')], [], [('Non-human', 'Animal', 'Bestiality(specify animal)', 'illegal  animal'), ('Non-human', 'Animal', 'Human Pet(Cat', ' Dog  Horse) Pet  role  serve  animal'), ('Non-human', 'Animal', 'Pony Girl_Boy', 'pony  animal'), ('Non-human', 'Animal', 'Zooerasty(sex with animals)', 'animal  illegal'), ('Non-human', 'Animal', 'Zoophilia  stroking_fondling of animals)', 'animal  fetish'), ('Non-human', 'Costume', 'cosplay', 'wear  role'), ('Non-human', 'Costume', 'Feathers and Fur play', 'Furt  wear  costume'), ('Non-human', 'Costume', 'Plushie Fetish', 'plushie  wear  role'), ('Non-human', 'Fantasy', 'Necrophilia(arousal by someone in corpse like state', ' or corpse) dead  illegal  fetish  fantasy  role'), ('Non-human', 'Fantasy', 'Giantess', 'giantess  large  role  fetish'), ('Non-human', 'Food fetish', 'Food Play', 'food  play  eat'), ('Non-human', 'Object', 'Human Furniture(foot stool', ' chair  table) human furniture  role  sub  serve'), ('Non-human', 'Object', 'Pygmalionism(sex with mannequins', ' statues  or blow-up dolls) mannequin'), ('Non-human', 'Role', 'Serving as an Art Object', 'art  serve  human  beauty'), ('Non-human', 'Role', 'Serving as an Ashtray', 'serve  D_S')], [('Oral Play', 'Testicles', 'Give', 'teabag  lick balls')], [('Oral Play', 'Testicles', 'Give', 'teabag  lick balls')], [('Other', 'Infibulation', '', ''), ('Other', 'Kali’s Teeth Bracelet', '', '')], [('Pain', '', 'Riding the Wooden Horse(crotch torture)', ''), ('Pain', 'Action', 'Belly Punching', 'belly  body pa'), ('Pain', 'Action', 'Crucifixion', 'spirit  cross  extreme'), ('Pain', 'Action', 'cupping', 'cup  cupping'), ('Pain', 'Action', 'edge play', 'cut  extreme  blood'), ('Pain', 'Action', 'Electric Play', 'electric  tens  magic wand'), ('Pain', 'Action', 'Slap', ' face hit  face  dom'), ('Pain', 'Action', 'Burn', 'pain  extreme  dom'), ('Pain', 'Action', 'Flogging', 'flog  impact'), ('Pain', 'Action', 'Genitorture', 'torture'), ('Pain', 'Action', 'Pinching', 'pinch  pain  finger'), ('Pain', 'Action', 'Pussy Whipping', 'whip  vagina  pain'), ('Pain', 'Action', 'Saline Injection_Infusion(breasts', ' labia  testicles-specify) needle  pain'), ('Pain', 'Action', 'Scratch', 'scratch'), ('Pain', 'Action', 'slapping', 'action  slap'), ('Pain', 'Action', 'Spanking(bare-hand)', 'spank  hand  action'), ('Pain', 'Breast', 'Breast Flagellation', 'whip  breast  dom'), ('Pain', 'Breast', 'breast_nipple torture', 'breast  nipple  pain  bite  dom'), ('Pain', 'Foot', 'Bastinado(beating the soles of feet)', 'foot  pain'), ('Pain', 'General', 'Pain(medium)', 'medium'), ('Pain', 'General', 'Pain(mild)', 'mild'), ('Pain', 'General', 'Pain(severe)', 'extreme'), ('Pain', 'Hair', 'Hair Pulling', 'hair  pull  role'), ('Pain', 'Hair', 'Wax Hair Removal', 'wax  hair  removal  pain  hot'), ('Pain', 'Modify', 'Body Piercing(temp)', 'pierce  modify  pain'), ('Pain', 'Modify', 'Bruises', 'bruise  modify  pain'), ('Pain', 'Modify', 'cell popping', 'cell  cell pop  heat'), ('Pain', 'Modify', 'Clitoris Piercing(temp)', 'pierce  clitoris  vagina'), ('Pain', 'Modify', 'cutting', 'modify  cut  blood'), ('Pain', 'Modify', 'Injections', 'needle  blood  risk'), ('Pain', 'Modify', 'Keloids(cutting and raising scars)', 'cut  blood  scar'), ('Pain', 'Modify', 'Labia Stretching', 'stretch  vagina'), ('Pain', 'Modify', 'Piercing(temp)', 'pierce  pain  modify'), ('Pain', 'Modify', 'Scarification', 'scar  modify  extreme  cut  burn'), ('Pain', 'Mouth', 'biting', 'mouth  oral  bite  pain'), ('Pain', 'Punish', 'Canes(caning as punishment)', 'cane  punish  role  dom'), ('Pain', 'Role', 'Pain Slut', 'Role  role play  pain'), ('Pain', 'Spank', 'Over the Knee Spanking', 'OTK  spank  knee  dom'), ('Pain', 'Spank', 'Spank', 'spank  pain  role'), ('Pain', 'Testicle', 'Ball_Testicle Stretching', 'ball  pain'), ('Pain', 'Testicle', 'Blue Balls(ball torture)', 'torture  ball  testicle'), ('Pain', 'Testicle', 'cock and ball torture', 'CBT  dom'), ('Pain', 'Toy', 'Battery Play(9 Volt)', 'battery  pain'), ('Pain', 'Toy', 'candle wax', 'wax  hot  pain  sensation  dom'), ('Pain', 'Toy', 'Canes', 'cane  toy  impact'), ('Pain', 'Toy', 'Cattle Prod', 'prod  toy  extreme  dom'), ('Pain', 'Toy', 'Clothespins', 'peg  pins  squeeze'), ('Pain', 'Toy', 'fire play', 'extreme  fire'), ('Pain', 'Toy', 'Genital Clamps', 'genital  toy  clamp'), ('Pain', 'Toy', 'Hairbrushes(spanked with)', 'spank  toy'), ('Pain', 'Toy', 'hooks', 'hook  extreme  pain  dom'), ('Pain', 'Toy', 'Hot Oils', 'oil  heat  sensation'), ('Pain', 'Toy', 'Hot Waxing', 'wax  heat  sensation'), ('Pain', 'Toy', 'Knife Play', 'extreme  edge  cut  dom  role'), ('Pain', 'Toy', 'Labia Clips', 'labia  clip  peg'), ('Pain', 'Toy', 'Needle Play', 'needle  pain'), ('Pain', 'Toy', 'Nipple Clamps', 'nipple  breast'), ('Pain', 'Toy', 'Paddles(leather)', 'paddle  impact  hit'), ('Pain', 'Toy', 'Paddles(studded)', 'paddle  impact'), ('Pain', 'Toy', 'Paddles(wooden)', ''), ('Pain', 'Toy', 'Riding Crops', 'horse  pony  crop  toy'), ('Pain', 'Toy', 'Single-Tail Whip', 'whip  toy  single-tail'), ('Pain', 'Toy', 'Spanking(belt', ' leather strap) belt  spank  toy'), ('Pain', 'Toy', 'Spanking with implement', 'spank  toy  belt  whip'), ('Pain', 'Toy', 'Strapping(full body beating)', 'strap '), ('Pain', 'Toy', 'Violet Wand', 'violet wand  toy  pain'), ('Pain', 'Toy', 'Water Torture', 'water  pain  punish  dom'), ('Pain', 'Toy', 'Wax Play', 'wax  toy  pain'), ('Pain', 'Toy', 'Whipping', 'Whip  toy  pain  impact')], [('Power', 'forced bisex', 'Receiving', 'force'), ('Power', 'given away to dom', ' temp', 'Given Away to Another Dominant(temp) '), ('Power', 'given to dom', ' perm', 'Given Away to Another Dominant(permanent) '), ('Power', 'Submissive', 'Begging', 'sub'), ('Power', 'Blackmail_Financial Subjection', 'mental', ''), ('Power', 'Boot Worship', 'shoe', ''), ('Power', 'Branding or Cauterization Branding', 'mark', ''), ('Power', 'Chauffeuring', 'dom', ' role'), ('Power', 'collar_leash', 'toy', ' ds'), ('Power', 'Consensual Non-Consensuality', 'force', ''), ('Power', 'degradation', 'object', ''), ('Power', 'Exercise(forced)', 'dom', ' role'), ('Power', 'Eye Contact Restrictions', 'dom', ' role'), ('Power', 'Fantasy Rape', 'fantasy', ' force'), ('Power', 'fear', 'mental', ''), ('Power', 'Following Orders', 'serve', ' rules'), ('Power', 'Forced Bedwetting', 'WS', ' f'), ('Power', 'Forced Domestic Service', 'roles', ' dom  f'), ('Power', 'Forced Dressing', 'crossdress', ' f'), ('Power', 'Forced Feminization', 'role', ' f'), ('Power', 'Forced Heterosexuality', 'orientation', ' f'), ('Power', 'Forced Homosexuality', 'orientation', ' f'), ('Power', 'Forced Masturbation', 'force', ' touch'), ('Power', 'Forced Nudity(private)', 'exh', ' f  pr'), ('Power', 'Forced Nudity(with group)', 'exh', ' f'), ('Power', 'Forced Oral Sex', 'oral', ' force'), ('Power', 'Forced Panty_Pants Wetting', 'WS', ' force'), ('Power', 'Forced Servitude', 'force sub', ''), ('Power', 'Gun Play', 'toy', ' extreme'), ('Power', 'Have clothing chosen for you', 'sub', ' roles'), ('Power', 'Having food chosen for you', 'submit', ' roles'), ('Power', 'Head Shaving', 'hair', ' permanent'), ('Power', 'Humiliation(private)', 'humiliate', ''), ('Power', 'Humiliation(public)', 'humiliate', ''), ('Power', 'Hypnotized Men', 'hypnotize', ' mental'), ('Power', 'Initiations _ Gauntlets', 'activity', ''), ('Power', 'Interrogations', 'talk', ''), ('Power', 'Lectures for Misbehavior', 'talk', ' femdom'), ('Power', 'Licking(floor)', 'lick floor', ''), ('Power', 'Mind Control', 'mental', ''), ('Power', 'Mind Fucking', 'mental', ''), ('Power', 'Name Change(for scene)', 'role', ''), ('Power', 'Name Change(legal', ' permanent)', 'sub  roles'), ('Power', 'Name Change(online)', 'dom', ' roles'), ('Power', 'obedience training', 'sub', ' roles'), ('Power', 'objectification', 'obect', ''), ('Power', 'Orgasm Control', 'org control', ''), ('Power', 'Orgasm on Command', 'dom orgasm', ''), ('Power', 'play rape', 'force', ''), ('Power', 'power exchange', 'power', ' brief'), ('Power', 'Prostitution(forced)', 'force', ''), ('Power', 'Queening(forcing vagina into mouth and nose)', 'femdom', ''), ('Power', 'Rape and Abduction Fantasy', 'fantasy', ''), ('Power', 'Restrictive rules on Behavior', 'dom', ' rules'), ('Power', 'sadism', 'sadism', ''), ('Power', 'Sadomasochism', 'SM', ''), ('Power', 'Serving Other Dom(supervised)', 'serve other', ''), ('Power', 'Serving Other Dom(unsupervised)', 'serve', ' no watch'), ('Power', 'Shaving(below neck)', 'hair', ' body'), ('Power', 'Speaking Restrictions', 'role', ' act'), ('Power', 'Standing In Corner', 'punishment', ''), ('Power', 'Submission(private)', 'sub', ' priv'), ('Power', 'Supplying new partners for Dom', 'serve', ' public'), ('Power', 'Switching', 'role', ' ds  mild'), ('Power', 'Teasing', 'mental', ' mild  play'), ('Power', 'Top_bottom', 'DS mild', ''), ('Power', 'Topping from the bottom', 'role', ' ds  mild'), ('Power', 'Total Power Exchange', 'role', ' ext'), ('Power', 'Use of non-verbal Safe words', 'role', ' acts'), ('Power', 'Use of Safe Words', 'role', ' talk'), ('Power', 'Verbal Abuse', 'role', ' talk'), ('Power', 'Verbal Humiliation', 'humiliate', ' talk'), ('Power', 'Weight Gain(enforced)', 'role', ' act  f'), ('Power', 'Weight Loss(enforced)', 'role', ' act  f'), ('Power', 'Wrestling(other subs)', 'activity', ''), ('Power', 'Wrestling(your partner)', 'force', '')], [('Restraint', 'Bondage(duct tapes)', 'tape', ' bondage'), ('Restraint', 'Bondage(heavy)', '', ''), ('Restraint', 'Bondage(Japanese)', '', ''), ('Restraint', 'Bondage(light)', 'bondage', ' gentle'), ('Restraint', 'Bondage(long', ' extended time)', 'bondage  extreme'), ('Restraint', 'Bondage(private setting)', 'private', ' bondage'), ('Restraint', 'Bondage(public setting', ' under clothes)', 'public  bondage'), ('Restraint', 'Bondage(ropes)', 'rope', ' bondage'), ('Restraint', 'Bondage(steel', ' wire', ' metal) wire  steel')], [('Restraint_Deprivation', 'Bondage(cuffs)', 'cuff', ' bondage'), ('Restraint_Deprivation', 'Sleeves', ' arm or leg(arm binders)', 'sleeve  wear'), ('Restraint_Deprivation', 'Gag', ' ball', 'gag'), ('Restraint_Deprivation', 'Bathroom Use Control', 'bathroom', 'body fluid  scat  urine  role'), ('Restraint_Deprivation', 'Confine', 'confine', ''), ('Restraint_Deprivation', 'blindfolds', 'blindfold', ' eye  see'), ('Restraint_Deprivation', 'Body Bag', 'bag', ''), ('Restraint_Deprivation', 'Breast_Chest Bondage', 'chest', ' breast  bondage'), ('Restraint_Deprivation', 'Breath Control_Play', 'breath', ' extreme'), ('Restraint_Deprivation', 'Cages', 'cage', ''), ('Restraint_Deprivation', 'Cells_Closets_Confined Spaces(locked in)', 'confine', ''), ('Restraint_Deprivation', 'chains', 'chains', ' toy'), ('Restraint_Deprivation', 'Chair Bondage', 'chair', ' toy'), ('Restraint_Deprivation', 'chastity devices', 'toy', ' role  control  chastity  orgasm control  toy'), ('Restraint_Deprivation', 'Cuffs(leather)', 'cuff', ' leather'), ('Restraint_Deprivation', 'Gags', ' retractor', 'medical  gag'), ('Restraint_Deprivation', 'Binding', ' foot', 'bind'), ('Restraint_Deprivation', 'Foot Bondage', 'foot', ''), ('Restraint_Deprivation', 'Hood', ' full', 'hood'), ('Restraint_Deprivation', 'Gags(cloth)', 'gag', ''), ('Restraint_Deprivation', 'Gags(dental)', 'gag', ' medical'), ('Restraint_Deprivation', 'Gags(phallic)', 'dildo', ' gag'), ('Restraint_Deprivation', 'Gags(scarf)', 'scarf', ' normal'), ('Restraint_Deprivation', 'Gags(tape)', 'tap', ' bind  gag'), ('Restraint_Deprivation', 'Gas Mask', 'gas mask', ' mask'), ('Restraint_Deprivation', 'Cuff', ' handcuff', 'cuff  hand  handcuff'), ('Restraint_Deprivation', 'Harnessing(leather)', 'leather', ' harness'), ('Restraint_Deprivation', 'Harnessing(rope)', 'rope', ' harness'), ('Restraint_Deprivation', 'Bind', ' head', 'head  bind'), ('Restraint_Deprivation', 'hoods', ' partial', 'hood'), ('Restraint_Deprivation', 'Immobilization', 'bind', ''), ('Restraint_Deprivation', 'Kneeling', 'kneel', ' role  dom'), ('Restraint_Deprivation', 'Lead on Leash', 'toy', ' leash  dom  role'), ('Restraint_Deprivation', 'Leather', 'leather', ''), ('Restraint_Deprivation', 'Sleeve_Binder', ' Leg', 'sleeve  bind  leg'), ('Restraint_Deprivation', 'Manacles', ' Shackles', ' leg irons bind'), ('Restraint_Deprivation', 'Mummification', 'immobilize', ''), ('Restraint_Deprivation', 'Plastic Wrap', 'toy', ' wrap  saran'), ('Restraint_Deprivation', 'Punishment(discipline)', 'punish', ' restraint  deprive'), ('Restraint_Deprivation', 'Restraints', 'general', ' restraints'), ('Restraint_Deprivation', 'Sensory Deprivation', 'deprive', ''), ('Restraint_Deprivation', 'Sexual Deprivation', 'orgasm control', ' dom'), ('Restraint_Deprivation', 'Shibari', 'bondage', ' japanese  shibari'), ('Restraint_Deprivation', 'Sleep Deprivation', 'sleep', ' mental'), ('Restraint_Deprivation', 'Sack', ' sleep', 'sleep  sack'), ('Restraint_Deprivation', 'Bondage', ' Sleeping(heavy)', 'sleep'), ('Restraint_Deprivation', 'Stocks_Pillory', 'stocks', ' pillory'), ('Restraint_Deprivation', 'Straight Jackets', 'jacket', ' immobilize  wear  clothing'), ('Restraint_Deprivation', 'Suspension(horizontal)', 'suspend', ''), ('Restraint_Deprivation', 'Suspension(inverted)', 'suspend', ''), ('Restraint_Deprivation', 'Suspension(upright)', 'suspend', ''), ('Restraint_Deprivation', 'Suspension by hook', 'suspend', ' hook'), ('Restraint_Deprivation', 'Suspension partially touching floor(on toes)', 'suspend', ''), ('Restraint_Deprivation', 'Thumb', 'thumb', ' part'), ('Restraint_Deprivation', 'Cuff', ' thumb', 'cuff  thumb'), ('Restraint_Deprivation', 'Body Bag', ' vacuum dage Bed', 'vacuum'), ('Restraint_Deprivation', 'Spreader Bars', 'spreader', ' spreader bar  toy')], [('Role Play', 'Group', 'Harems(belonging to)', 'harem  group  role'), ('Role Play', 'Group', 'Harems(serving with others)', 'harem  group  serve'), ('Role Play', 'role', ' extreme', 'Prostitution(real) prostitute  risk'), ('Role Play', 'safe word', 'Safe Call', 'safe word  safe  role')], [('Roles', '24_7', '', ''), ('Roles', 'age play', '', ''), ('Roles', 'BBW(Big Beautiful Women)', 'fantasy', ' body'), ('Roles', 'Bicourious', 'orientation', ''), ('Roles', 'Bimboification', 'role', ' fantasy'), ('Roles', 'bisexuality', 'orientation', ''), ('Roles', 'Breeding', 'fantasy', ''), ('Roles', 'Butch_Femme Role Play', 'role', ' orientation  gender'), ('Roles', 'Castration Fantasy', 'fantasy', ' ext'), ('Roles', 'Cross-Dressing_Gender bending', 'orientation', ''), ('Roles', 'cuckold', 'others', ' ext'), ('Roles', 'Damsel in Distress Fetish', 'sub', ' role'), ('Roles', 'doctor_nurse play', 'medical', ''), ('Roles', 'Examinations(medical)', 'medical', ''), ('Roles', 'Examinations(physical)', 'test', ' act'), ('Roles', 'Fantasy Abandonment', 'deprivation', ''), ('Roles', 'Fantasy Abduction', 'force', ' fant'), ('Roles', 'Fantasy Gang Rape', 'force', ' fant'), ('Roles', 'Female Body Builders', 'fantasy', ''), ('Roles', 'Fighting_Catfight', 'force', ' group'), ('Roles', 'gorean', 'slave', ''), ('Roles', 'Infantilism', 'ABDL', ' role  WS'), ('Roles', 'Leatherman_woman', 'leather', ''), ('Roles', 'masks', 'clothing', ''), ('Roles', 'Medical Play', 'medical', ''), ('Roles', 'Personality Modification', 'role', ' ext'), ('Roles', 'Phone Sex(commercial provider)', 'talk', ''), ('Roles', 'Phone Sex(serving Dom)', '', ''), ('Roles', 'Phone Sex(serving Dom’s friends)', 'talk', ' sub  oth'), ('Roles', 'Prison Scenes', 'scene', ' fant'), ('Roles', 'Prostitution(fantasy)', 'force', ' fant'), ('Roles', 'Punishment Scene', 'pain', ' fant'), ('Roles', 'Religious Scenes', 'fantasy', ''), ('Roles', 'Serving as Maid_Butler', 'serve', ' role'), ('Roles', 'Serving as Waitress_Waiter', 'role', ''), ('Roles', 'Sexual Addiction', 'fantasy', ''), ('Roles', 'Sexual intercourse with older person(Gerontophilia)', 'age', ''), ('Roles', 'sissification', 'orientation', ''), ('Roles', 'sleep', 'activity', ''), ('Roles', 'smoking', 'smoke', ''), ('Roles', 'tranny', 'orientation', ''), ('Roles', 'Vore(eating ppl_things)', 'fetish', '')], [('Wearing', 'Jewelry', 'Piercing', ' Clitoris  permanent '), ('Wearing', 'Lingerie', 'Corsets', ''), ('Wearing', 'costumes', '', ''), ('Wearing', 'High Heel Worship', '', ''), ('Wearing', 'Hoods', '', ''), ('Wearing', 'Latex Clothing', '', ''), ('Wearing', 'Leather Clothing', '', ''), ('Wearing', 'Lingerie(on your partner)', '', ''), ('Wearing', 'Lingerie(wearing)', '', ''), ('Wearing', 'Liquid Rubber', '', ''), ('Wearing', 'Men in Underwear', '', ''), ('Wearing', 'Nipple Rings(piercing)', '', ''), ('Wearing', 'panties', '', ''), ('Wearing', 'pantyhose', '', ''), ('Wearing', 'Rubber', ' Gummi', ' Latex  Spandex  Wetsuits  Fetish '), ('Wearing', 'Scarves and Mask Fetish', '', ''), ('Wearing', 'Slutty Clothing(private)', '', ''), ('Wearing', 'Spandex Clothing', '', ''), ('Wearing', 'Tattoos(permanent)', '', ''), ('Wearing', 'Uniforms', '', ''), ('Wearing', 'Wearing Boots', 'foot', ' boot  shoe  foot fetish'), ('Wearing', 'Wearing Symbolic Jewelry', '', ''), ('Wearing', 'Wearing Uniforms(Military', ' Fireman', ' Police  Sport  Nazi  etc.)')], [('Toys', 'medical', 'Speculums(vaginal)', 'speculum'), ('Toys', 'Slings_Swings', 'movement', ' swing'), ('Toys', 'The Humbler', '', '')], [('Watching_Showing', 'Dance', ' watch', 'Belly Dance '), ('Watching_Showing', 'Dance', ' watch', 'Erotic '), ('Watching_Showing', 'Media', ' Amateur', 'Watch movie '), ('Watching_Showing', 'Media', ' Amateur', 'View pictures  artistic '), ('Watching_Showing', 'Media', ' Amateur', 'Take pictures  artistic '), ('Watching_Showing', 'Media', ' Amateur', 'Take pictures  dirty '), ('Watching_Showing', 'Media', ' Amateur', 'Receiving '), ('Watching_Showing', 'Media', ' Amateur', 'Video Taped Scenes '), ('Watching_Showing', 'Media', ' Erotica', 'Read '), ('Watching_Showing', 'Public', 'Wear collar', ''), ('Watching_Showing', 'Public', ' indoors', 'Swingers Club  no interaction '), ('Watching_Showing', 'Public', ' show', 'Known '), ('Watching_Showing', 'Public', ' show', 'Strangers  individual or small '), ('Watching_Showing', 'Public', ' show', 'Public Sex  rural '), ('Watching_Showing', 'Public', ' show', 'Public Sex '), ('Watching_Showing', 'Public', ' show', 'Slutty Clothing(public) '), ('Watching_Showing', 'Public', ' sneak', 'Outdoor Sceneing '), ('Watching_Showing', 'Public', ' sneak', 'Secret Sex in Public '), ('Watching_Showing', 'Public', ' touch', 'Masturbation  strangers '), ('Watching_Showing', 'Public', ' touch', 'Masturbation  Partner '), ('Watching_Showing', 'Public', ' touch', 'Masturbation  known '), ('Watching_Showing', 'Submission(public)', '', ''), ('Watching_Showing', 'Video Porn_BDSM watching', '', ''), ('Watching_Showing', 'voyeurism', '', ''), ('Watching_Showing', 'Voyeurism(watching partner with others)', '', '')], [('Wearing', 'Jewelry', 'Piercing', ' Clitoris  permanent '), ('Wearing', 'Lingerie', 'Corsets', ''), ('Wearing', 'costumes', '', ''), ('Wearing', 'High Heel Worship', '', ''), ('Wearing', 'Hoods', '', ''), ('Wearing', 'Latex Clothing', '', ''), ('Wearing', 'Leather Clothing', '', ''), ('Wearing', 'Lingerie(on your partner)', '', ''), ('Wearing', 'Lingerie(wearing)', '', ''), ('Wearing', 'Liquid Rubber', '', ''), ('Wearing', 'Men in Underwear', '', ''), ('Wearing', 'Nipple Rings(piercing)', '', ''), ('Wearing', 'panties', '', ''), ('Wearing', 'pantyhose', '', ''), ('Wearing', 'Rubber', ' Gummi', ' Latex  Spandex  Wetsuits  Fetish '), ('Wearing', 'Scarves and Mask Fetish', '', ''), ('Wearing', 'Slutty Clothing(private)', '', ''), ('Wearing', 'Spandex Clothing', '', ''), ('Wearing', 'Tattoos(permanent)', '', ''), ('Wearing', 'Uniforms', '', ''), ('Wearing', 'Wearing Boots', 'foot', ' boot  shoe  foot fetish'), ('Wearing', 'Wearing Symbolic Jewelry', '', ''), ('Wearing', 'Wearing Uniforms(Military', ' Fireman', ' Police  Sport  Nazi  etc.)')]]
    
    [t for T in Titles for t in T]





    [('Body Fluids', 'Blood', 'Touching', 'blood play'),
     ('Body Fluids', 'Blood', 'Seeing', 'blood  cut  knife'),
     ('Body Fluids', 'Blood', 'Tasting', 'blood  fluid  partner  risk '),
     ('Body Fluids', 'Blood', 'Receiving', 'blood  risk  inside'),
     ('Body Fluids', 'Blood', ' menses', 'Cunnalingus lick period  fetish'),
     ('Body Fluids', 'Blood', ' menses', 'Intercourse period'),
     ('Body Fluids', 'Breast milk', 'Sucking_drinking', 'milk  drink'),
     ('Body Fluids', 'Breast milk', 'see_observe', 'lactate'),
     ('Body Fluids', 'Diapers', 'wearing', 'scat  urine'),
     ('Body Fluids', 'Female ejaculation', 'Giving', 'make squirt'),
     ('Body Fluids', 'Female Ejaculation', 'drink', 'squirt'),
     ('Body Fluids', 'Gas', 'Farting', 'fart(smell  do)'),
     ('Body Fluids', 'Human Toilet', 'Giving', 'scat  pee  role'),
     ('Body Fluids', 'saliva', 'Spitting(in mouth)', 'mouth'),
     ('Body Fluids', 'saliva', 'Spitting(on body)', 'body  dom'),
     ('Body Fluids', 'Scat', 'Defecate in diaper', 'diaper  ABDL'),
     ('Body Fluids', 'Scat', 'Animal on face', 'animal  non-human  scat  receive'),
     ('Body Fluids', 'Scat', 'play', 'scat'),
     ('Body Fluids', 'scat', 'Giving', 'scat  pooping'),
     ('Body Fluids', 'Scat', 'defecating in underpants', 'panties  poop'),
     ('Body Fluids', 'scat', 'Human toilet', ''),
     ('Body Fluids', 'Scat', 'swallowing', 'eat  scat'),
     ('Body Fluids', 'Semen', 'Swallowing', 'swallow  BJ'),
     ('Body Fluids', 'Semen', ' Bukkake', 'Receiving group  bukkake'),
     ('Body Fluids', 'Semen', ' creampie', 'Giving creampie  vaginal  sex'),
     ('Body Fluids', 'tears', ' crying', 'watching cry  tears'),
     ('Body Fluids', 'Toy', 'Litter Box', 'scat  urine  role  non-human'),
     ('Body Fluids', 'Toy', ' pot', 'Chamber Pot Use WS  scat  dom  power'),
     ('Body Fluids', 'Toys', ' catheter', 'Inserting sound  medical'),
     ('Body Fluids', 'Unusual scents', 'Smelling', 'smell  scent'),
     ('Body Fluids', 'Urine', 'licking', 'cunnalingus  urine'),
     ('Body Fluids', 'Urine', 'sucking', 'BJ  urine'),
     ('Body Fluids', 'Urine', 'urinating on body(below neck)', ''),
     ('Body Fluids', 'Urine', 'urinating(on face_in mouth)', 'pee  mouth  give'),
     ('Body Fluids', 'Urine', 'Wetting underpants or clothes', 'urine  panties'),
     ('Body Fluids', 'Urine', 'Human toilet', 'role  toilet  non-human'),
     ('Body Fluids', 'Urine', 'drinking', 'urine  drink'),
     ('Body Fluids', 'Urine', 'Urinate in diaper', ''),
     ('Body Parts',
      'Breast',
      'Breast_Chest Fucking',
      'titty fuck  breast  sex  frot'),
     ('Body Parts', 'Foot', 'View', 'foot  watch'),
     ('Body Parts', 'Foot', 'Touch', ' during intercourse foot  fetish  sex'),
     ('Body Parts', 'Foot', 'Touch', ' not intercourse foot  standalone'),
     ('Body Parts', 'Foot', 'Toe Sucking', 'toe  foot'),
     ('Body Parts', 'Genitalia', ' Female', 'touch labia'),
     ('Body Parts', 'Genitalia', ' Female', 'Pussy pumping toy  pump'),
     ('Body Parts', 'Genitalia', ' Female', 'Worship slave  serve  femdom  pussy'),
     ('Body Parts', 'Genitals', 'Urethral Play', 'sounding  catheter'),
     ('Body Parts', 'Hair', 'body hair', 'bush'),
     ('Body Parts', 'Hair', 'Shaved Genitals(having)', 'repeat  non genita('),
     ('Body Parts', 'Hair', 'Shaved Genitals(on partner)', 'hair  shave  partner'),
     ('Body Parts', 'hair', 'Shaving', ' non-genitals shave  hair'),
     ('Body Parts', 'hand', 'Hand Licking', 'lick  hand'),
     ('Body Parts', 'Hands', 'Giving', 'handjob  hand'),
     ('Body Parts',
      'modify',
      'Anthropomorphic(self contrived or false amputation)',
      'extreme  amputate'),
     ('Body Parts',
      'Modify',
      ' extreme',
      'Plastic Surgery plastic surgery  watch'),
     ('Body Parts', 'Mouth', 'tongues', 'tongue'),
     ('Body Parts', 'Nipples', 'Touch', ' oral nipple  breast  lick'),
     ('Body Parts', 'Nipples', 'Touch', ' manual nipple  breast  rub nipple'),
     ('Body Parts', 'Other', 'armpit hair', 'hair  armpit  natural'),
     ('Body Parts', 'Other', 'Finger Sucking', 'finger  suck'),
     ('Body Parts', 'Penis', 'Cock Worship', 'penis  fetish'),
     ('Body Parts', 'Tattoo', ' temporary', 'Receiving modify  tattoo  body'),
     ('Body Parts',
      'urethra',
      'urethral play',
      'urethra  sounding  medical  catheter'),
     ('Butt Stuff',
      'Intercourse',
      ' permitted',
      'gentle  on partner anal  normal'),
     ('Butt Stuff', 'Intercourse', ' forced', 'forced  on partner anal  force'),
     ('Butt Stuff', 'Other', ' pain', 'on self ass torture  butt'),
     ('Butt Stuff', 'Other', ' Pain', 'on partner ass torture  butt'),
     ('Butt Stuff',
      'Part',
      ' finger_self',
      'penetrate  on self finger  butt  masterbate'),
     ('Butt Stuff',
      'Part',
      ' finger_partner',
      'penetrate partner finger  butt  partner'),
     ('Butt Stuff', 'Part', ' hand', 'Fisting(double) DP  hand'),
     ('Butt Stuff', 'Part', ' hand', 'penetrate  on self fisting  ass  self'),
     ('Butt Stuff',
      'Part',
      ' hand',
      'penetrate  on partner finger  butt  partner'),
     ('Butt Stuff', 'Part', ' penis', 'Giving anal  butt stuff  partner'),
     ('Butt Stuff', 'Part', ' penis', 'Receiving anal sex  fuck  partner  risk'),
     ('Butt Stuff', 'Part', ' prostate', 'Other prostate  butt  femdom'),
     ('Butt Stuff', 'Part', ' tongue', 'Rimming(anus licking) rim  butt  tongue'),
     ('Butt Stuff', 'Other', ' double penetration', 'double penetration DP  toy'),
     ('Butt Stuff',
      'Other',
      ' lick own semen',
      'licking own semen from anus of partner lick  ass  semen  body_fluid  butt_stuff  taste'),
     ('Butt Stuff',
      'Other',
      ' lick other semen',
      "licking others' semen from anus of partner butt stuff  body fluids  semen  lick  cuck  others  risk"),
     ('Butt Stuff',
      'Other',
      ' forced self toy',
      'forced  on self forced  dir  butt  toy'),
     ('Butt Stuff',
      'Other',
      ' forced partner toy',
      'forced  on partner forced  butt  toy'),
     ('Butt Stuff', 'Toy', ' dildo', 'Penetrate strapon  pegging  femdom'),
     ('Butt Stuff', 'Toy', ' dildo', 'Suck suck  strapon  femdom'),
     ('Butt Stuff', 'Toy', ' dildo', 'Give strapon  femdom  anal'),
     ('Butt Stuff', 'Toy', ' enema', 'Giving toy  enema'),
     ('Butt Stuff', 'Toy', ' enema', 'Receiving toy  enema'),
     ('Butt Stuff', 'Toy', ' self ice enema', 'Give toy  enema  cold  ice'),
     ('Butt Stuff', 'Toy', ' other ice enema', 'Ice Enemas toy  enema  cold  ice'),
     ('Butt Stuff', 'Toy', ' ginger', 'figging toy  pain'),
     ('Butt Stuff', 'Toy', ' medical', 'Speculums(anal) medical  butt  toy'),
     ('Butt Stuff', 'Toy', ' self beads', 'Receiving beads butt toys'),
     ('Butt Stuff', 'Toy', ' part beads', 'Giving beads  anal  toy'),
     ('Butt Stuff', 'Toy', ' other beads', 'Receive beads  anal  toy'),
     ('Butt Stuff', 'Toy', ' self dildo', 'Inserting toys  dildo dildo partner'),
     ('Butt Stuff', 'Toy', ' partner dildo', 'Wear toy  dildo'),
     ('Butt Stuff', 'Toy', ' partner plug', 'Inserting plug  toy'),
     ('Butt Stuff', 'Toy', ' partner strap-on', 'Giving strap_on  femdom  role'),
     ('Butt Stuff', 'Toy', ' self strap-on', 'Receiving '),
     ('D_S', 'Forced Exposure', 'Public Exposure', 'humiliate  public'),
     ('Groups', 'anonymous', 'strangers', 'group  orgy  anon'),
     ('Groups', 'Gentle', 'Flirting', 'gentle  talk  group'),
     ('Groups', 'Orgy', 'Gangbangs', 'force  group  role'),
     ('Groups', 'Orgy', 'Glory Hole', 'anon  orgy  cover'),
     ('Groups', 'Orgy', 'Orgy', ' general orgy'),
     ('Groups', 'orgy', 'Swapping(hard)', 'swap  group'),
     ('Groups',
      'orgy',
      'Triple Penetration(multiple partners)',
      'orgy  triple  TP'),
     ('Groups', 'swing', 'Swapping(soft)', 'swap  group'),
     ('Groups', 'swing', 'Swinging', 'swing'),
     ('Groups', 'threesome', 'threesomes', '3'),
     ('Non-human', 'Animal', 'Bestiality(specify animal)', 'illegal  animal'),
     ('Non-human',
      'Animal',
      'Human Pet(Cat',
      ' Dog  Horse) Pet  role  serve  animal'),
     ('Non-human', 'Animal', 'Pony Girl_Boy', 'pony  animal'),
     ('Non-human', 'Animal', 'Zooerasty(sex with animals)', 'animal  illegal'),
     ('Non-human',
      'Animal',
      'Zoophilia  stroking_fondling of animals)',
      'animal  fetish'),
     ('Non-human', 'Costume', 'cosplay', 'wear  role'),
     ('Non-human', 'Costume', 'Feathers and Fur play', 'Furt  wear  costume'),
     ('Non-human', 'Costume', 'Plushie Fetish', 'plushie  wear  role'),
     ('Non-human',
      'Fantasy',
      'Necrophilia(arousal by someone in corpse like state',
      ' or corpse) dead  illegal  fetish  fantasy  role'),
     ('Non-human', 'Fantasy', 'Giantess', 'giantess  large  role  fetish'),
     ('Non-human', 'Food fetish', 'Food Play', 'food  play  eat'),
     ('Non-human',
      'Object',
      'Human Furniture(foot stool',
      ' chair  table) human furniture  role  sub  serve'),
     ('Non-human',
      'Object',
      'Pygmalionism(sex with mannequins',
      ' statues  or blow-up dolls) mannequin'),
     ('Non-human',
      'Role',
      'Serving as an Art Object',
      'art  serve  human  beauty'),
     ('Non-human', 'Role', 'Serving as an Ashtray', 'serve  D_S'),
     ('Oral Play', 'Testicles', 'Give', 'teabag  lick balls'),
     ('Oral Play', 'Testicles', 'Give', 'teabag  lick balls'),
     ('Other', 'Infibulation', '', ''),
     ('Other', 'Kali’s Teeth Bracelet', '', ''),
     ('Pain', '', 'Riding the Wooden Horse(crotch torture)', ''),
     ('Pain', 'Action', 'Belly Punching', 'belly  body pa'),
     ('Pain', 'Action', 'Crucifixion', 'spirit  cross  extreme'),
     ('Pain', 'Action', 'cupping', 'cup  cupping'),
     ('Pain', 'Action', 'edge play', 'cut  extreme  blood'),
     ('Pain', 'Action', 'Electric Play', 'electric  tens  magic wand'),
     ('Pain', 'Action', 'Slap', ' face hit  face  dom'),
     ('Pain', 'Action', 'Burn', 'pain  extreme  dom'),
     ('Pain', 'Action', 'Flogging', 'flog  impact'),
     ('Pain', 'Action', 'Genitorture', 'torture'),
     ('Pain', 'Action', 'Pinching', 'pinch  pain  finger'),
     ('Pain', 'Action', 'Pussy Whipping', 'whip  vagina  pain'),
     ('Pain',
      'Action',
      'Saline Injection_Infusion(breasts',
      ' labia  testicles-specify) needle  pain'),
     ('Pain', 'Action', 'Scratch', 'scratch'),
     ('Pain', 'Action', 'slapping', 'action  slap'),
     ('Pain', 'Action', 'Spanking(bare-hand)', 'spank  hand  action'),
     ('Pain', 'Breast', 'Breast Flagellation', 'whip  breast  dom'),
     ('Pain',
      'Breast',
      'breast_nipple torture',
      'breast  nipple  pain  bite  dom'),
     ('Pain', 'Foot', 'Bastinado(beating the soles of feet)', 'foot  pain'),
     ('Pain', 'General', 'Pain(medium)', 'medium'),
     ('Pain', 'General', 'Pain(mild)', 'mild'),
     ('Pain', 'General', 'Pain(severe)', 'extreme'),
     ('Pain', 'Hair', 'Hair Pulling', 'hair  pull  role'),
     ('Pain', 'Hair', 'Wax Hair Removal', 'wax  hair  removal  pain  hot'),
     ('Pain', 'Modify', 'Body Piercing(temp)', 'pierce  modify  pain'),
     ('Pain', 'Modify', 'Bruises', 'bruise  modify  pain'),
     ('Pain', 'Modify', 'cell popping', 'cell  cell pop  heat'),
     ('Pain', 'Modify', 'Clitoris Piercing(temp)', 'pierce  clitoris  vagina'),
     ('Pain', 'Modify', 'cutting', 'modify  cut  blood'),
     ('Pain', 'Modify', 'Injections', 'needle  blood  risk'),
     ('Pain', 'Modify', 'Keloids(cutting and raising scars)', 'cut  blood  scar'),
     ('Pain', 'Modify', 'Labia Stretching', 'stretch  vagina'),
     ('Pain', 'Modify', 'Piercing(temp)', 'pierce  pain  modify'),
     ('Pain', 'Modify', 'Scarification', 'scar  modify  extreme  cut  burn'),
     ('Pain', 'Mouth', 'biting', 'mouth  oral  bite  pain'),
     ('Pain', 'Punish', 'Canes(caning as punishment)', 'cane  punish  role  dom'),
     ('Pain', 'Role', 'Pain Slut', 'Role  role play  pain'),
     ('Pain', 'Spank', 'Over the Knee Spanking', 'OTK  spank  knee  dom'),
     ('Pain', 'Spank', 'Spank', 'spank  pain  role'),
     ('Pain', 'Testicle', 'Ball_Testicle Stretching', 'ball  pain'),
     ('Pain', 'Testicle', 'Blue Balls(ball torture)', 'torture  ball  testicle'),
     ('Pain', 'Testicle', 'cock and ball torture', 'CBT  dom'),
     ('Pain', 'Toy', 'Battery Play(9 Volt)', 'battery  pain'),
     ('Pain', 'Toy', 'candle wax', 'wax  hot  pain  sensation  dom'),
     ('Pain', 'Toy', 'Canes', 'cane  toy  impact'),
     ('Pain', 'Toy', 'Cattle Prod', 'prod  toy  extreme  dom'),
     ('Pain', 'Toy', 'Clothespins', 'peg  pins  squeeze'),
     ('Pain', 'Toy', 'fire play', 'extreme  fire'),
     ('Pain', 'Toy', 'Genital Clamps', 'genital  toy  clamp'),
     ('Pain', 'Toy', 'Hairbrushes(spanked with)', 'spank  toy'),
     ('Pain', 'Toy', 'hooks', 'hook  extreme  pain  dom'),
     ('Pain', 'Toy', 'Hot Oils', 'oil  heat  sensation'),
     ('Pain', 'Toy', 'Hot Waxing', 'wax  heat  sensation'),
     ('Pain', 'Toy', 'Knife Play', 'extreme  edge  cut  dom  role'),
     ('Pain', 'Toy', 'Labia Clips', 'labia  clip  peg'),
     ('Pain', 'Toy', 'Needle Play', 'needle  pain'),
     ('Pain', 'Toy', 'Nipple Clamps', 'nipple  breast'),
     ('Pain', 'Toy', 'Paddles(leather)', 'paddle  impact  hit'),
     ('Pain', 'Toy', 'Paddles(studded)', 'paddle  impact'),
     ('Pain', 'Toy', 'Paddles(wooden)', ''),
     ('Pain', 'Toy', 'Riding Crops', 'horse  pony  crop  toy'),
     ('Pain', 'Toy', 'Single-Tail Whip', 'whip  toy  single-tail'),
     ('Pain', 'Toy', 'Spanking(belt', ' leather strap) belt  spank  toy'),
     ('Pain', 'Toy', 'Spanking with implement', 'spank  toy  belt  whip'),
     ('Pain', 'Toy', 'Strapping(full body beating)', 'strap '),
     ('Pain', 'Toy', 'Violet Wand', 'violet wand  toy  pain'),
     ('Pain', 'Toy', 'Water Torture', 'water  pain  punish  dom'),
     ('Pain', 'Toy', 'Wax Play', 'wax  toy  pain'),
     ('Pain', 'Toy', 'Whipping', 'Whip  toy  pain  impact'),
     ('Power', 'forced bisex', 'Receiving', 'force'),
     ('Power',
      'given away to dom',
      ' temp',
      'Given Away to Another Dominant(temp) '),
     ('Power',
      'given to dom',
      ' perm',
      'Given Away to Another Dominant(permanent) '),
     ('Power', 'Submissive', 'Begging', 'sub'),
     ('Power', 'Blackmail_Financial Subjection', 'mental', ''),
     ('Power', 'Boot Worship', 'shoe', ''),
     ('Power', 'Branding or Cauterization Branding', 'mark', ''),
     ('Power', 'Chauffeuring', 'dom', ' role'),
     ('Power', 'collar_leash', 'toy', ' ds'),
     ('Power', 'Consensual Non-Consensuality', 'force', ''),
     ('Power', 'degradation', 'object', ''),
     ('Power', 'Exercise(forced)', 'dom', ' role'),
     ('Power', 'Eye Contact Restrictions', 'dom', ' role'),
     ('Power', 'Fantasy Rape', 'fantasy', ' force'),
     ('Power', 'fear', 'mental', ''),
     ('Power', 'Following Orders', 'serve', ' rules'),
     ('Power', 'Forced Bedwetting', 'WS', ' f'),
     ('Power', 'Forced Domestic Service', 'roles', ' dom  f'),
     ('Power', 'Forced Dressing', 'crossdress', ' f'),
     ('Power', 'Forced Feminization', 'role', ' f'),
     ('Power', 'Forced Heterosexuality', 'orientation', ' f'),
     ('Power', 'Forced Homosexuality', 'orientation', ' f'),
     ('Power', 'Forced Masturbation', 'force', ' touch'),
     ('Power', 'Forced Nudity(private)', 'exh', ' f  pr'),
     ('Power', 'Forced Nudity(with group)', 'exh', ' f'),
     ('Power', 'Forced Oral Sex', 'oral', ' force'),
     ('Power', 'Forced Panty_Pants Wetting', 'WS', ' force'),
     ('Power', 'Forced Servitude', 'force sub', ''),
     ('Power', 'Gun Play', 'toy', ' extreme'),
     ('Power', 'Have clothing chosen for you', 'sub', ' roles'),
     ('Power', 'Having food chosen for you', 'submit', ' roles'),
     ('Power', 'Head Shaving', 'hair', ' permanent'),
     ('Power', 'Humiliation(private)', 'humiliate', ''),
     ('Power', 'Humiliation(public)', 'humiliate', ''),
     ('Power', 'Hypnotized Men', 'hypnotize', ' mental'),
     ('Power', 'Initiations _ Gauntlets', 'activity', ''),
     ('Power', 'Interrogations', 'talk', ''),
     ('Power', 'Lectures for Misbehavior', 'talk', ' femdom'),
     ('Power', 'Licking(floor)', 'lick floor', ''),
     ('Power', 'Mind Control', 'mental', ''),
     ('Power', 'Mind Fucking', 'mental', ''),
     ('Power', 'Name Change(for scene)', 'role', ''),
     ('Power', 'Name Change(legal', ' permanent)', 'sub  roles'),
     ('Power', 'Name Change(online)', 'dom', ' roles'),
     ('Power', 'obedience training', 'sub', ' roles'),
     ('Power', 'objectification', 'obect', ''),
     ('Power', 'Orgasm Control', 'org control', ''),
     ('Power', 'Orgasm on Command', 'dom orgasm', ''),
     ('Power', 'play rape', 'force', ''),
     ('Power', 'power exchange', 'power', ' brief'),
     ('Power', 'Prostitution(forced)', 'force', ''),
     ('Power', 'Queening(forcing vagina into mouth and nose)', 'femdom', ''),
     ('Power', 'Rape and Abduction Fantasy', 'fantasy', ''),
     ('Power', 'Restrictive rules on Behavior', 'dom', ' rules'),
     ('Power', 'sadism', 'sadism', ''),
     ('Power', 'Sadomasochism', 'SM', ''),
     ('Power', 'Serving Other Dom(supervised)', 'serve other', ''),
     ('Power', 'Serving Other Dom(unsupervised)', 'serve', ' no watch'),
     ('Power', 'Shaving(below neck)', 'hair', ' body'),
     ('Power', 'Speaking Restrictions', 'role', ' act'),
     ('Power', 'Standing In Corner', 'punishment', ''),
     ('Power', 'Submission(private)', 'sub', ' priv'),
     ('Power', 'Supplying new partners for Dom', 'serve', ' public'),
     ('Power', 'Switching', 'role', ' ds  mild'),
     ('Power', 'Teasing', 'mental', ' mild  play'),
     ('Power', 'Top_bottom', 'DS mild', ''),
     ('Power', 'Topping from the bottom', 'role', ' ds  mild'),
     ('Power', 'Total Power Exchange', 'role', ' ext'),
     ('Power', 'Use of non-verbal Safe words', 'role', ' acts'),
     ('Power', 'Use of Safe Words', 'role', ' talk'),
     ('Power', 'Verbal Abuse', 'role', ' talk'),
     ('Power', 'Verbal Humiliation', 'humiliate', ' talk'),
     ('Power', 'Weight Gain(enforced)', 'role', ' act  f'),
     ('Power', 'Weight Loss(enforced)', 'role', ' act  f'),
     ('Power', 'Wrestling(other subs)', 'activity', ''),
     ('Power', 'Wrestling(your partner)', 'force', ''),
     ('Restraint', 'Bondage(duct tapes)', 'tape', ' bondage'),
     ('Restraint', 'Bondage(heavy)', '', ''),
     ('Restraint', 'Bondage(Japanese)', '', ''),
     ('Restraint', 'Bondage(light)', 'bondage', ' gentle'),
     ('Restraint', 'Bondage(long', ' extended time)', 'bondage  extreme'),
     ('Restraint', 'Bondage(private setting)', 'private', ' bondage'),
     ('Restraint', 'Bondage(public setting', ' under clothes)', 'public  bondage'),
     ('Restraint', 'Bondage(ropes)', 'rope', ' bondage'),
     ('Restraint', 'Bondage(steel', ' wire', ' metal) wire  steel'),
     ('Restraint_Deprivation', 'Bondage(cuffs)', 'cuff', ' bondage'),
     ('Restraint_Deprivation',
      'Sleeves',
      ' arm or leg(arm binders)',
      'sleeve  wear'),
     ('Restraint_Deprivation', 'Gag', ' ball', 'gag'),
     ('Restraint_Deprivation',
      'Bathroom Use Control',
      'bathroom',
      'body fluid  scat  urine  role'),
     ('Restraint_Deprivation', 'Confine', 'confine', ''),
     ('Restraint_Deprivation', 'blindfolds', 'blindfold', ' eye  see'),
     ('Restraint_Deprivation', 'Body Bag', 'bag', ''),
     ('Restraint_Deprivation',
      'Breast_Chest Bondage',
      'chest',
      ' breast  bondage'),
     ('Restraint_Deprivation', 'Breath Control_Play', 'breath', ' extreme'),
     ('Restraint_Deprivation', 'Cages', 'cage', ''),
     ('Restraint_Deprivation',
      'Cells_Closets_Confined Spaces(locked in)',
      'confine',
      ''),
     ('Restraint_Deprivation', 'chains', 'chains', ' toy'),
     ('Restraint_Deprivation', 'Chair Bondage', 'chair', ' toy'),
     ('Restraint_Deprivation',
      'chastity devices',
      'toy',
      ' role  control  chastity  orgasm control  toy'),
     ('Restraint_Deprivation', 'Cuffs(leather)', 'cuff', ' leather'),
     ('Restraint_Deprivation', 'Gags', ' retractor', 'medical  gag'),
     ('Restraint_Deprivation', 'Binding', ' foot', 'bind'),
     ('Restraint_Deprivation', 'Foot Bondage', 'foot', ''),
     ('Restraint_Deprivation', 'Hood', ' full', 'hood'),
     ('Restraint_Deprivation', 'Gags(cloth)', 'gag', ''),
     ('Restraint_Deprivation', 'Gags(dental)', 'gag', ' medical'),
     ('Restraint_Deprivation', 'Gags(phallic)', 'dildo', ' gag'),
     ('Restraint_Deprivation', 'Gags(scarf)', 'scarf', ' normal'),
     ('Restraint_Deprivation', 'Gags(tape)', 'tap', ' bind  gag'),
     ('Restraint_Deprivation', 'Gas Mask', 'gas mask', ' mask'),
     ('Restraint_Deprivation', 'Cuff', ' handcuff', 'cuff  hand  handcuff'),
     ('Restraint_Deprivation', 'Harnessing(leather)', 'leather', ' harness'),
     ('Restraint_Deprivation', 'Harnessing(rope)', 'rope', ' harness'),
     ('Restraint_Deprivation', 'Bind', ' head', 'head  bind'),
     ('Restraint_Deprivation', 'hoods', ' partial', 'hood'),
     ('Restraint_Deprivation', 'Immobilization', 'bind', ''),
     ('Restraint_Deprivation', 'Kneeling', 'kneel', ' role  dom'),
     ('Restraint_Deprivation', 'Lead on Leash', 'toy', ' leash  dom  role'),
     ('Restraint_Deprivation', 'Leather', 'leather', ''),
     ('Restraint_Deprivation', 'Sleeve_Binder', ' Leg', 'sleeve  bind  leg'),
     ('Restraint_Deprivation', 'Manacles', ' Shackles', ' leg irons bind'),
     ('Restraint_Deprivation', 'Mummification', 'immobilize', ''),
     ('Restraint_Deprivation', 'Plastic Wrap', 'toy', ' wrap  saran'),
     ('Restraint_Deprivation',
      'Punishment(discipline)',
      'punish',
      ' restraint  deprive'),
     ('Restraint_Deprivation', 'Restraints', 'general', ' restraints'),
     ('Restraint_Deprivation', 'Sensory Deprivation', 'deprive', ''),
     ('Restraint_Deprivation', 'Sexual Deprivation', 'orgasm control', ' dom'),
     ('Restraint_Deprivation', 'Shibari', 'bondage', ' japanese  shibari'),
     ('Restraint_Deprivation', 'Sleep Deprivation', 'sleep', ' mental'),
     ('Restraint_Deprivation', 'Sack', ' sleep', 'sleep  sack'),
     ('Restraint_Deprivation', 'Bondage', ' Sleeping(heavy)', 'sleep'),
     ('Restraint_Deprivation', 'Stocks_Pillory', 'stocks', ' pillory'),
     ('Restraint_Deprivation',
      'Straight Jackets',
      'jacket',
      ' immobilize  wear  clothing'),
     ('Restraint_Deprivation', 'Suspension(horizontal)', 'suspend', ''),
     ('Restraint_Deprivation', 'Suspension(inverted)', 'suspend', ''),
     ('Restraint_Deprivation', 'Suspension(upright)', 'suspend', ''),
     ('Restraint_Deprivation', 'Suspension by hook', 'suspend', ' hook'),
     ('Restraint_Deprivation',
      'Suspension partially touching floor(on toes)',
      'suspend',
      ''),
     ('Restraint_Deprivation', 'Thumb', 'thumb', ' part'),
     ('Restraint_Deprivation', 'Cuff', ' thumb', 'cuff  thumb'),
     ('Restraint_Deprivation', 'Body Bag', ' vacuum dage Bed', 'vacuum'),
     ('Restraint_Deprivation', 'Spreader Bars', 'spreader', ' spreader bar  toy'),
     ('Role Play', 'Group', 'Harems(belonging to)', 'harem  group  role'),
     ('Role Play', 'Group', 'Harems(serving with others)', 'harem  group  serve'),
     ('Role Play', 'role', ' extreme', 'Prostitution(real) prostitute  risk'),
     ('Role Play', 'safe word', 'Safe Call', 'safe word  safe  role'),
     ('Roles', '24_7', '', ''),
     ('Roles', 'age play', '', ''),
     ('Roles', 'BBW(Big Beautiful Women)', 'fantasy', ' body'),
     ('Roles', 'Bicourious', 'orientation', ''),
     ('Roles', 'Bimboification', 'role', ' fantasy'),
     ('Roles', 'bisexuality', 'orientation', ''),
     ('Roles', 'Breeding', 'fantasy', ''),
     ('Roles', 'Butch_Femme Role Play', 'role', ' orientation  gender'),
     ('Roles', 'Castration Fantasy', 'fantasy', ' ext'),
     ('Roles', 'Cross-Dressing_Gender bending', 'orientation', ''),
     ('Roles', 'cuckold', 'others', ' ext'),
     ('Roles', 'Damsel in Distress Fetish', 'sub', ' role'),
     ('Roles', 'doctor_nurse play', 'medical', ''),
     ('Roles', 'Examinations(medical)', 'medical', ''),
     ('Roles', 'Examinations(physical)', 'test', ' act'),
     ('Roles', 'Fantasy Abandonment', 'deprivation', ''),
     ('Roles', 'Fantasy Abduction', 'force', ' fant'),
     ('Roles', 'Fantasy Gang Rape', 'force', ' fant'),
     ('Roles', 'Female Body Builders', 'fantasy', ''),
     ('Roles', 'Fighting_Catfight', 'force', ' group'),
     ('Roles', 'gorean', 'slave', ''),
     ('Roles', 'Infantilism', 'ABDL', ' role  WS'),
     ('Roles', 'Leatherman_woman', 'leather', ''),
     ('Roles', 'masks', 'clothing', ''),
     ('Roles', 'Medical Play', 'medical', ''),
     ('Roles', 'Personality Modification', 'role', ' ext'),
     ('Roles', 'Phone Sex(commercial provider)', 'talk', ''),
     ('Roles', 'Phone Sex(serving Dom)', '', ''),
     ('Roles', 'Phone Sex(serving Dom’s friends)', 'talk', ' sub  oth'),
     ('Roles', 'Prison Scenes', 'scene', ' fant'),
     ('Roles', 'Prostitution(fantasy)', 'force', ' fant'),
     ('Roles', 'Punishment Scene', 'pain', ' fant'),
     ('Roles', 'Religious Scenes', 'fantasy', ''),
     ('Roles', 'Serving as Maid_Butler', 'serve', ' role'),
     ('Roles', 'Serving as Waitress_Waiter', 'role', ''),
     ('Roles', 'Sexual Addiction', 'fantasy', ''),
     ('Roles', 'Sexual intercourse with older person(Gerontophilia)', 'age', ''),
     ('Roles', 'sissification', 'orientation', ''),
     ('Roles', 'sleep', 'activity', ''),
     ('Roles', 'smoking', 'smoke', ''),
     ('Roles', 'tranny', 'orientation', ''),
     ('Roles', 'Vore(eating ppl_things)', 'fetish', ''),
     ('Wearing', 'Jewelry', 'Piercing', ' Clitoris  permanent '),
     ('Wearing', 'Lingerie', 'Corsets', ''),
     ('Wearing', 'costumes', '', ''),
     ('Wearing', 'High Heel Worship', '', ''),
     ('Wearing', 'Hoods', '', ''),
     ('Wearing', 'Latex Clothing', '', ''),
     ('Wearing', 'Leather Clothing', '', ''),
     ('Wearing', 'Lingerie(on your partner)', '', ''),
     ('Wearing', 'Lingerie(wearing)', '', ''),
     ('Wearing', 'Liquid Rubber', '', ''),
     ('Wearing', 'Men in Underwear', '', ''),
     ('Wearing', 'Nipple Rings(piercing)', '', ''),
     ('Wearing', 'panties', '', ''),
     ('Wearing', 'pantyhose', '', ''),
     ('Wearing', 'Rubber', ' Gummi', ' Latex  Spandex  Wetsuits  Fetish '),
     ('Wearing', 'Scarves and Mask Fetish', '', ''),
     ('Wearing', 'Slutty Clothing(private)', '', ''),
     ('Wearing', 'Spandex Clothing', '', ''),
     ('Wearing', 'Tattoos(permanent)', '', ''),
     ('Wearing', 'Uniforms', '', ''),
     ('Wearing', 'Wearing Boots', 'foot', ' boot  shoe  foot fetish'),
     ('Wearing', 'Wearing Symbolic Jewelry', '', ''),
     ('Wearing',
      'Wearing Uniforms(Military',
      ' Fireman',
      ' Police  Sport  Nazi  etc.)'),
     ('Toys', 'medical', 'Speculums(vaginal)', 'speculum'),
     ('Toys', 'Slings_Swings', 'movement', ' swing'),
     ('Toys', 'The Humbler', '', ''),
     ('Watching_Showing', 'Dance', ' watch', 'Belly Dance '),
     ('Watching_Showing', 'Dance', ' watch', 'Erotic '),
     ('Watching_Showing', 'Media', ' Amateur', 'Watch movie '),
     ('Watching_Showing', 'Media', ' Amateur', 'View pictures  artistic '),
     ('Watching_Showing', 'Media', ' Amateur', 'Take pictures  artistic '),
     ('Watching_Showing', 'Media', ' Amateur', 'Take pictures  dirty '),
     ('Watching_Showing', 'Media', ' Amateur', 'Receiving '),
     ('Watching_Showing', 'Media', ' Amateur', 'Video Taped Scenes '),
     ('Watching_Showing', 'Media', ' Erotica', 'Read '),
     ('Watching_Showing', 'Public', 'Wear collar', ''),
     ('Watching_Showing', 'Public', ' indoors', 'Swingers Club  no interaction '),
     ('Watching_Showing', 'Public', ' show', 'Known '),
     ('Watching_Showing', 'Public', ' show', 'Strangers  individual or small '),
     ('Watching_Showing', 'Public', ' show', 'Public Sex  rural '),
     ('Watching_Showing', 'Public', ' show', 'Public Sex '),
     ('Watching_Showing', 'Public', ' show', 'Slutty Clothing(public) '),
     ('Watching_Showing', 'Public', ' sneak', 'Outdoor Sceneing '),
     ('Watching_Showing', 'Public', ' sneak', 'Secret Sex in Public '),
     ('Watching_Showing', 'Public', ' touch', 'Masturbation  strangers '),
     ('Watching_Showing', 'Public', ' touch', 'Masturbation  Partner '),
     ('Watching_Showing', 'Public', ' touch', 'Masturbation  known '),
     ('Watching_Showing', 'Submission(public)', '', ''),
     ('Watching_Showing', 'Video Porn_BDSM watching', '', ''),
     ('Watching_Showing', 'voyeurism', '', ''),
     ('Watching_Showing', 'Voyeurism(watching partner with others)', '', ''),
     ('Wearing', 'Jewelry', 'Piercing', ' Clitoris  permanent '),
     ('Wearing', 'Lingerie', 'Corsets', ''),
     ('Wearing', 'costumes', '', ''),
     ('Wearing', 'High Heel Worship', '', ''),
     ('Wearing', 'Hoods', '', ''),
     ('Wearing', 'Latex Clothing', '', ''),
     ('Wearing', 'Leather Clothing', '', ''),
     ('Wearing', 'Lingerie(on your partner)', '', ''),
     ('Wearing', 'Lingerie(wearing)', '', ''),
     ('Wearing', 'Liquid Rubber', '', ''),
     ('Wearing', 'Men in Underwear', '', ''),
     ('Wearing', 'Nipple Rings(piercing)', '', ''),
     ('Wearing', 'panties', '', ''),
     ('Wearing', 'pantyhose', '', ''),
     ('Wearing', 'Rubber', ' Gummi', ' Latex  Spandex  Wetsuits  Fetish '),
     ('Wearing', 'Scarves and Mask Fetish', '', ''),
     ('Wearing', 'Slutty Clothing(private)', '', ''),
     ('Wearing', 'Spandex Clothing', '', ''),
     ('Wearing', 'Tattoos(permanent)', '', ''),
     ('Wearing', 'Uniforms', '', ''),
     ('Wearing', 'Wearing Boots', 'foot', ' boot  shoe  foot fetish'),
     ('Wearing', 'Wearing Symbolic Jewelry', '', ''),
     ('Wearing',
      'Wearing Uniforms(Military',
      ' Fireman',
      ' Police  Sport  Nazi  etc.)')]




    Master = {'ButtStuff': [['Part', ' finger', 'penetrate', ' on self', 'finger', ' butt', ' masterbate'], ['Part', ' finger', 'penetrate partner', 'finger', ' butt', ' partner'], ['Part', ' hand', 'penetrate', ' on partner', 'finger', ' butt', ' partner'], ['Part', ' hand', 'penetrate', ' on self', 'fisting', ' ass', ' self'], ['Part', ' hand', 'Fisting double', 'DP', ' hand'], ['Part', ' penis', 'Giving', 'anal', ' butt stuff', ' partner'], ['Part', ' penis', 'Receiving', 'anal sex', ' fuck', ' partner', ' risk'], ['Part', ' prostate', 'Other', 'prostate', ' butt', ' femdom'], ['Part', ' tongue', 'Rimming anus licking', 'rim', ' butt', ' tongue'], ['Part', ' toy', 'double penetration', 'DP', ' toy'], ['Toy', ' dildo', 'Give', 'strapon', ' femdom', ' anal'], ['Toy', ' dildo', 'Penetrate', 'strapon', ' pegging', ' femdom'], ['Toy', ' dildo', 'Suck', 'suck', ' strapon', ' femdom'], ['Toy', ' enema', 'Giving', 'toy', ' enema'], ['Toy', ' enema', 'Receiving', 'toy', ' enema'], ['Toy', ' enema', ' ice', 'Give', 'toy', ' enema', ' cold', ' ice'], ['Toy', ' enema', ' ice', 'Ice Enemas', 'toy', ' enema', ' cold', ' ice'], ['Toy', ' ginger', 'figging', 'toy', ' pain'], ['Toy', ' medical', 'Speculums anal', 'medical', ' butt', ' toy'], ['toys', ' beads', 'Giving', 'beads', ' anal', ' toy'], ['Toys', ' beads', 'Receive', 'beads', ' anal', ' toy'], ['Toys', ' beads', 'Receiving', 'beads butt toys'], ['Toys', ' dildo', 'Inserting', 'toys', ' dildo dildo partner'], ['toys', ' dildo', 'Wear', 'toy', ' dildo'], ['Toys', ' plug', 'Inserting', 'plug', ' toy'], ['Toys', ' strap-on', 'Giving', 'strap_on', ' femdom', ' role'], ['Toys', ' strap-on', 'Receiving', 'None'], ['anal sex', 'forced', ' on partner', 'anal', ' force'], ['anal sex', 'gentle', ' on partner', 'anal', ' normal'], ['pain play', 'on partner', 'ass torture', ' butt'], ['pain play', 'on self', 'ass torture', ' butt'], ['semen', 'licking others semen from anus of partner', 'butt stuff', ' body fluids', ' semen', ' lick', ' cuck', ' others', ' risk'], ['semen', 'licking own semen from anus of partner', 'lick', ' ass', ' semen', ' body_fluid', ' butt_stuff', ' taste'], ['toy', 'forced', ' on partner', 'forced', ' butt', ' toy'], ['toy', 'forced', ' on self', 'forced', ' dir', ' butt', ' toy']], 'NonHuman': [['Animal', 'Human Pet Cat', ' Dog', ' Horse', 'Pet', ' role', ' serve', ' animal'], ['Animal', 'Bestiality specify animal', 'illegal', ' animal'], ['Animal', 'Pony GirlBoy', 'pony', ' animal'], ['Animal', 'Zooerasty sex with animals', 'animal', ' illegal'], ['Animal', 'Zoophilia  strokingfondling of animals', 'animal', ' fetish'], ['Costume', 'cosplay', 'wear', ' role'], ['Costume', 'Feathers and Fur play', 'Furt', ' wear', ' costume'], ['Costume', 'Plushie Fetish', 'plushie', ' wear', ' role'], ['Fantasy', 'Necrophilia arousal by someone in corpse like state', ' or corpse', 'dead', ' illegal', ' fetish', ' fantasy', ' role'], ['Fantasy', 'Giantess', 'giantess', ' large', ' role', ' fetish'], ['Food fetish', 'Food Play', 'food', ' play', ' eat'], ['Object', 'Human Furniture foot stool', ' chair', ' table', 'human furniture', ' role', ' sub', ' serve'], ['Object', 'Pygmalionism sex with mannequins', ' statues', ' or blow-up dolls', 'mannequin'], ['Role', 'Serving as an Art Object', 'art', ' serve', ' human', ' beauty'], ['Role', 'Serving as an Ashtray', 'serve', ' DS']], 'Pain': [['Action', 'Saline InjectionInfusion breasts', ' labia', ' testicles-specify', 'needle', ' pain'], ['Action', 'Slap', ' face', 'hit', ' face', ' dom'], ['Action', 'Belly Punching', 'belly', ' body pa'], ['Action', 'Burn', 'pain', ' extreme', ' dom'], ['Action', 'Crucifixion', 'spirit', ' cross', ' extreme'], ['Action', 'cupping', 'cup', ' cupping'], ['Action', 'edge play', 'cut', ' extreme', ' blood'], ['Action', 'Electric Play', 'electric', ' tens', ' magic wand'], ['Action', 'Flogging', 'flog', ' impact'], ['Action', 'Genitorture', 'torture'], ['Action', 'Pinching', 'pinch', ' pain', ' finger'], ['Action', 'Pussy Whipping', 'whip', ' vagina', ' pain'], ['Action', 'Scratch', 'scratch'], ['Action', 'slapping', 'action', ' slap'], ['Action', 'Spanking bare-hand', 'spank', ' hand', ' action'], ['Breast', 'Breast Flagellation', 'whip', ' breast', ' dom'], ['Breast', 'breastnipple torture', 'breast', ' nipple', ' pain', ' bite', ' dom'], ['Foot', 'Bastinado beating the soles of feet', 'foot', ' pain'], ['General', 'Pain medium', 'medium'], ['General', 'Pain mild', 'mild'], ['General', 'Pain severe', 'extreme'], ['Hair', 'Hair Pulling', 'hair', ' pull', ' role'], ['Hair', 'Wax Hair Removal', 'wax', ' hair', ' removal', ' pain', ' hot'], ['Modify', 'Body Piercing temp', 'pierce', ' modify', ' pain'], ['Modify', 'Bruises', 'bruise', ' modify', ' pain'], ['Modify', 'cell popping', 'cell', ' cell pop', ' heat'], ['Modify', 'Clitoris Piercing temp', 'pierce', ' clitoris', ' vagina'], ['Modify', 'cutting', 'modify', ' cut', ' blood'], ['Modify', 'Injections', 'needle', ' blood', ' risk'], ['Modify', 'Keloids cutting and raising scars', 'cut', ' blood', ' scar'], ['Modify', 'Labia Stretching', 'stretch', ' vagina'], ['Modify', 'Piercing temp', 'pierce', ' pain', ' modify'], ['Modify', 'Scarification', 'scar', ' modify', ' extreme', ' cut', ' burn'], ['Mouth', 'biting', 'mouth', ' oral', ' bite', ' pain'], ['None', 'Riding the Wooden Horse crotch torture', 'None'], ['Punish', 'Canes caning as punishment', 'cane', ' punish', ' role', ' dom'], ['Role', 'Pain Slut', 'Role', ' role play', ' pain'], ['Spank', 'Over the Knee Spanking', 'OTK', ' spank', ' knee', ' dom'], ['Spank', 'Spank', 'spank', ' pain', ' role'], ['Testicle', 'BallTesticle Stretching', 'ball', ' pain'], ['Testicle', 'Blue Balls ball torture', 'torture', ' ball', ' testicle'], ['Testicle', 'cock and ball torture', 'CBT', ' dom'], ['Toy', 'Spanking belt', ' leather strap', 'belt', ' spank', ' toy'], ['Toy', 'Battery Play 9 Volt', 'battery', ' pain'], ['Toy', 'candle wax', 'wax', ' hot', ' pain', ' sensation', ' dom'], ['Toy', 'Canes', 'cane', ' toy', ' impact'], ['Toy', 'Cattle Prod', 'prod', ' toy', ' extreme', ' dom'], ['Toy', 'Clothespins', 'peg', ' pins', ' squeeze'], ['Toy', 'fire play', 'extreme', ' fire'], ['Toy', 'Genital Clamps', 'genital', ' toy', ' clamp'], ['Toy', 'Hairbrushes spanked with', 'spank', ' toy'], ['Toy', 'hooks', 'hook', ' extreme', ' pain', ' dom'], ['Toy', 'Hot Oils', 'oil', ' heat', ' sensation'], ['Toy', 'Hot Waxing', 'wax', ' heat', ' sensation'], ['Toy', 'Knife Play', 'extreme', ' edge', ' cut', ' dom', ' role'], ['Toy', 'Labia Clips', 'labia', ' clip', ' peg'], ['Toy', 'Needle Play', 'needle', ' pain'], ['Toy', 'Nipple Clamps', 'nipple', ' breast'], ['Toy', 'Paddles leather', 'paddle', ' impact', ' hit'], ['Toy', 'Paddles studded', 'paddle', ' impact'], ['Toy', 'Paddles wooden', 'None'], ['Toy', 'Riding Crops', 'horse', ' pony', ' crop', ' toy'], ['Toy', 'Single-Tail Whip', 'whip', ' toy', ' single-tail'], ['Toy', 'Spanking with implement', 'spank', ' toy', ' belt', ' whip'], ['Toy', 'Strapping full body beating', 'strap', ' ?'], ['Toy', 'Violet Wand', 'violet wand', ' toy', ' pain'], ['Toy', 'Water Torture', 'water', ' pain', ' punish', ' dom'], ['Toy', 'Wax Play', 'wax', ' toy', ' pain'], ['Toy', 'Whipping', 'Whip', ' toy', ' pain', ' impact']], 'Wearing': [['Jewelry', 'Piercing', ' Clitoris', ' permanent', 'None'], ['Lingerie', 'Corsets', 'None'], ['None', 'Rubber', ' Gummi', ' Latex', ' Spandex', ' Wetsuits', ' Fetish', 'None'], ['None', 'Wearing Uniforms Military', ' Fireman', ' Police', ' Sport', ' Nazi', ' etc.', 'None'], ['None', 'costumes', 'None'], ['None', 'High Heel Worship', 'None'], ['None', 'Hoods', 'None'], ['None', 'Latex Clothing', 'None'], ['None', 'Leather Clothing', 'None'], ['None', 'Lingerie on your partner', 'None'], ['None', 'Lingerie wearing', 'None'], ['None', 'Liquid Rubber', 'None'], ['None', 'Men in Underwear', 'None'], ['None', 'Nipple Rings piercing', 'None'], ['None', 'panties', 'None'], ['None', 'pantyhose', 'None'], ['None', 'Scarves and Mask Fetish', 'None'], ['None', 'Slutty Clothing private', 'None'], ['None', 'Spandex Clothing', 'None'], ['None', 'Tattoos permanent', 'None'], ['None', 'Uniforms', 'None'], ['None', 'Wearing Boots', 'foot', ' boot', ' shoe', ' foot fetish'], ['None', 'Wearing Symbolic Jewelry', 'None']], 'OralPlay': [['Foot', 'Foot Licking', 'foot fetish', ' foot worship'], ['Lick', 'Licking people', 'random', ' risk', ' illegal'], ['Object', 'Licking toilet bowl', 'toilet', ' lick', ' DS'], ['Penis', 'Deep Throating penis', 'BJ', ' oral', ' penis'], ['Sex', 'face fucking', 'force', ' oral'], ['Testicles', 'Give', 'teabag', ' lick balls'], ['Toy', 'Deep throating strap on', 'BJ', ' femdom', ' toy'], ['Vagina', 'Facesit', 'femdom', ' vagina', ' cunnalingus'], ['Vagina', 'Giving', 'cunnalingus', ' giving']], 'RestraintDeprivation': [['None', 'Bondage long', ' extended time', 'bondage', ' extreme'], ['None', 'Bondage public setting', ' under clothes', 'public', ' bondage'], ['None', 'Bondage steel', ' wire', ' metal', 'wire', ' steel'], ['None', 'Bondage duct tapes', 'tape', ' bondage'], ['None', 'Bondage heavy', 'None'], ['None', 'Bondage Japanese', 'None'], ['None', 'Bondage light', 'bondage', ' gentle'], ['None', 'Bondage private setting', 'private', ' bondage'], ['None', 'Bondage ropes', 'rope', ' bondage'], ['None', 'Bind', ' head', 'head', ' bind'], ['None', 'Binding', ' foot', 'bind'], ['None', 'Body Bag', ' vacuum dage Bed', 'vacuum'], ['None', 'Bondage', ' Sleeping heavy', 'sleep'], ['None', 'Cuff', ' handcuff', 'cuff', ' hand', ' handcuff'], ['None', 'Cuff', ' thumb', 'cuff', ' thumb'], ['None', 'Gag', ' ball', 'gag'], ['None', 'Gags', ' retractor', 'medical', ' gag'], ['None', 'Hood', ' full', 'hood'], ['None', 'hoods', ' partial', 'hood'], ['None', 'Manacles', ' Shackles', ' leg irons', 'bind'], ['None', 'Sack', ' sleep', 'sleep', ' sack'], ['None', 'SleeveBinder', ' Leg', 'sleeve', ' bind', ' leg'], ['None', 'Sleeves', ' arm or leg arm binders', 'sleeve', ' wear'], ['None', 'Bathroom Use Control', 'bathroom', 'body fluid', ' scat', ' urine', ' role'], ['None', 'blindfolds', 'blindfold', ' eye', ' see'], ['None', 'Body Bag', 'bag'], ['None', 'Bondage cuffs', 'cuff', ' bondage'], ['None', 'BreastChest Bondage', 'chest', ' breast', ' bondage'], ['None', 'Breath ControlPlay', 'breath', ' extreme'], ['None', 'Cages', 'cage'], ['None', 'CellsClosetsConfined Spaces locked in', 'confine'], ['None', 'chains', 'chains', ' toy'], ['None', 'Chair Bondage', 'chair', ' toy'], ['None', 'chastity devices', 'toy', ' role', ' control', ' chastity', ' orgasm control', ' toy'], ['None', 'Confine', 'confine'], ['None', 'Cuffs leather', 'cuff', ' leather'], ['None', 'Foot Bondage', 'foot'], ['None', 'Gags cloth', 'gag'], ['None', 'Gags dental', 'gag', ' medical'], ['None', 'Gags phallic', 'dildo', ' gag'], ['None', 'Gags scarf', 'scarf', ' normal'], ['None', 'Gags tape', 'tap', ' bind', ' gag'], ['None', 'Gas Mask', 'gas mask', ' mask'], ['None', 'Harnessing leather', 'leather', ' harness'], ['None', 'Harnessing rope', 'rope', ' harness'], ['None', 'Immobilization', 'bind'], ['None', 'Kneeling', 'kneel', ' role', ' dom'], ['None', 'Lead on Leash', 'toy', ' leash', ' dom', ' role'], ['None', 'Leather', 'leather'], ['None', 'Mummification', 'immobilize'], ['None', 'Plastic Wrap', 'toy', ' wrap', ' saran'], ['None', 'Punishment discipline', 'punish', ' restraint', ' deprive'], ['None', 'Restraints', 'general', ' restraints'], ['None', 'Sensory Deprivation', 'deprive'], ['None', 'Sexual Deprivation', 'orgasm control', ' dom'], ['None', 'Shibari', 'bondage', ' japanese', ' shibari'], ['None', 'Sleep Deprivation', 'sleep', ' mental'], ['None', 'Spreader Bars', 'spreader', ' spreader bar', ' toy'], ['None', 'StocksPillory', 'stocks', ' pillory'], ['None', 'Straight Jackets', 'jacket', ' immobilize', ' wear', ' clothing'], ['None', 'Suspension horizontal', 'suspend'], ['None', 'Suspension inverted', 'suspend'], ['None', 'Suspension upright', 'suspend'], ['None', 'Suspension by hook', 'suspend', ' hook'], ['None', 'Suspension partially touching floor on toes', 'suspend'], ['None', 'Thumb', 'thumb', ' part']], 'BodyFluids': [['tears', ' crying', 'watching', 'cry', ' tears'], ['Toy', ' pot', 'Chamber Pot Use', 'WS', ' scat', ' dom', ' power'], ['Toys', ' catheter', 'Inserting', 'sound', ' medical'], ['Blood', 'Receiving', 'blood', ' risk', ' inside'], ['Blood', 'Seeing', 'blood', ' cut', ' knife'], ['Blood', 'Tasting', 'blood', ' fluid', ' partner', ' risk', ''], ['Blood', 'Touching', 'blood play'], ['Breast milk', 'seeobserve', 'lactate'], ['Breast milk', 'Suckingdrinking', 'milk', ' drink'], ['Diapers', 'wearing', 'scat', ' urine'], ['Female Ejaculation', 'drink', 'squirt'], ['Female ejaculation', 'Giving', 'make squirt'], ['Gas', 'Farting', 'fart smell', ' do'], ['Human Toilet', 'Giving', 'scat', ' pee', ' role'], ['saliva', 'Spitting in mouth', 'mouth'], ['saliva', 'Spitting on body', 'body', ' dom'], ['Scat', 'Animal on face', 'animal', ' non-human', ' scat', ' receive'], ['Scat', 'Defecate in diaper', 'diaper', ' ABDL'], ['Scat', 'defecating in underpants', 'panties', ' poop'], ['scat', 'Giving', 'scat', ' pooping'], ['scat', 'Human toilet', 'None'], ['Scat', 'play', 'scat'], ['Scat', 'swallowing', 'eat', ' scat'], ['Semen', 'Swallowing', 'swallow', ' BJ'], ['Toy', 'Litter Box', 'scat', ' urine', ' role', ' non-human'], ['Unusual scents', 'Smelling', 'smell', ' scent'], ['Urine', 'drinking', 'urine', ' drink'], ['Urine', 'Human toilet', 'role', ' toilet', ' non-human'], ['Urine', 'licking', 'cunnalingus', ' urine'], ['Urine', 'sucking', 'BJ', ' urine'], ['Urine', 'Urinate in diaper', 'None'], ['Urine', 'urinating on facein mouth', 'pee', ' mouth', ' give'], ['Urine', 'urinating on body below neck', 'None'], ['Urine', 'Wetting underpants or clothes', 'urine', ' panties']], 'Power': [['given away to dom', ' temp', 'Given Away to Another Dominant temp', 'None'], ['given to dom', ' perm', 'Given Away to Another Dominant permanent', 'None'], ['forced bisex', 'Receiving', 'force'], ['None', 'Name Change legal', ' permanent', 'sub', ' roles'], ['None', 'BlackmailFinancial Subjection', 'mental'], ['None', 'Boot Worship', 'shoe'], ['None', 'Branding or Cauterization Branding', 'mark'], ['Forced Exposure', 'Public Exposure', 'humiliate', ' public'], ['None', 'Chauffeuring', 'dom', ' role'], ['None', 'collarleash', 'toy', ' ds'], ['None', 'Consensual Non-Consensuality', 'force'], ['None', 'degradation', 'object'], ['None', 'Exercise forced', 'dom', ' role'], ['None', 'Eye Contact Restrictions', 'dom', ' role'], ['None', 'Fantasy Rape', 'fantasy', ' force'], ['None', 'fear', 'mental'], ['None', 'Following Orders', 'serve', ' rules'], ['None', 'Forced Bedwetting', 'WS', ' f'], ['None', 'Forced Domestic Service', 'roles', ' dom', ' f'], ['None', 'Forced Dressing', 'crossdress', ' f'], ['None', 'Forced Feminization', 'role', ' f'], ['None', 'Forced Heterosexuality', 'orientation', ' f'], ['None', 'Forced Homosexuality', 'orientation', ' f'], ['None', 'Forced Masturbation', 'force', ' touch'], ['None', 'Forced Nudity private', 'exh', ' f', ' pr'], ['None', 'Forced Nudity with group', 'exh', ' f'], ['None', 'Forced Oral Sex', 'oral', ' force'], ['None', 'Forced PantyPants Wetting', 'WS', ' force'], ['None', 'Forced Servitude', 'force sub'], ['None', 'Gun Play', 'toy', ' extreme'], ['None', 'Have clothing chosen for you', 'sub', ' roles'], ['None', 'Having food chosen for you', 'submit', ' roles'], ['None', 'Head Shaving', 'hair', ' permanent'], ['None', 'Humiliation private', 'humiliate'], ['None', 'Humiliation public', 'humiliate'], ['None', 'Hypnotized Men', 'hypnotize', ' mental'], ['None', 'Initiations  Gauntlets', 'activity'], ['None', 'Interrogations', 'talk'], ['None', 'Lectures for Misbehavior', 'talk', ' femdom'], ['None', 'Licking floor', 'lick floor'], ['None', 'Mind Control', 'mental'], ['None', 'Mind Fucking', 'mental'], ['None', 'Name Change for scene', 'role'], ['None', 'Name Change online', 'dom', ' roles'], ['None', 'obedience training', 'sub', ' roles'], ['None', 'objectification', 'obect'], ['None', 'Orgasm Control', 'org control'], ['None', 'Orgasm on Command', 'dom orgasm'], ['None', 'play rape', 'force'], ['None', 'power exchange', 'power', ' brief'], ['None', 'Prostitution forced', 'force'], ['None', 'Queening forcing vagina into mouth and nose', 'femdom'], ['None', 'Rape and Abduction Fantasy', 'fantasy'], ['None', 'Restrictive rules on Behavior', 'dom', ' rules'], ['None', 'sadism', 'sadism'], ['None', 'Sadomasochism', 'SM'], ['None', 'Serving Other Dom supervised', 'serve other'], ['None', 'Serving Other Dom unsupervised', 'serve', ' no watch'], ['None', 'Shaving below neck', 'hair', ' body'], ['None', 'Speaking Restrictions', 'role', ' act'], ['None', 'Standing In Corner', 'punishment'], ['None', 'Submission private', 'sub', ' priv'], ['None', 'Supplying new partners for Dom', 'serve', ' public'], ['None', 'Switching', 'role', ' ds', ' mild'], ['None', 'Teasing', 'mental', ' mild', ' play'], ['None', 'Topbottom', 'DS mild'], ['None', 'Topping from the bottom', 'role', ' ds', ' mild'], ['None', 'Total Power Exchange', 'role', ' ext'], ['None', 'Use of non-verbal Safe words', 'role', ' acts'], ['None', 'Use of Safe Words', 'role', ' talk'], ['None', 'Verbal Abuse', 'role', ' talk'], ['None', 'Verbal Humiliation', 'humiliate', ' talk'], ['None', 'Weight Gain enforced', 'role', ' act', ' f'], ['None', 'Weight Loss enforced', 'role', ' act', ' f'], ['None', 'Wrestling other subs', 'activity'], ['None', 'Wrestling your partner', 'force'], [' Submissive', 'Begging', 'sub']], 'MentalStuff': [['Spirituality', 'Kundalini Chi', 'spirit', ' chi'], ['Spirituality', 'O Kee Pa the Sun Dance', 'None', ' spirituality'], ['Spirituality', 'Sacred SexSacred Sexuality', 'spirit', ' mental'], ['Spirituality', 'Tantra – Tantric and Taoist Sex', 'tao', ' tantra'], ['Spirituality', 'tantra', 'tantra']], 'Roles': [['role', ' extreme', 'Prostitution real', 'prostitute', ' risk'], ['Group', 'Harems belonging to', 'harem', ' group', ' role'], ['Group', 'Harems serving with others', 'harem', ' group', ' serve'], ['safe word', 'Safe Call', 'safe word', ' safe', ' role'], ['None', '247', 'None'], ['None', 'age play', 'None'], ['None', 'BBW Big Beautiful Women', 'fantasy', ' body'], ['None', 'Bicourious', 'orientation'], ['None', 'Bimboification', 'role', ' fantasy'], ['None', 'bisexuality', 'orientation'], ['None', 'Breeding', 'fantasy'], ['None', 'ButchFemme Role Play', 'role', ' orientation', ' gender'], ['None', 'Castration Fantasy', 'fantasy', ' ext'], ['None', 'Cross-DressingGender bending', 'orientation'], ['None', 'cuckold', 'others', ' ext'], ['None', 'Damsel in Distress Fetish', 'sub', ' role'], ['None', 'doctornurse play', 'medical'], ['None', 'Examinations medical', 'medical'], ['None', 'Examinations physical', 'test', ' act'], ['None', 'Fantasy Abandonment', 'deprivation'], ['None', 'Fantasy Abduction', 'force', ' fant'], ['None', 'Fantasy Gang Rape', 'force', ' fant'], ['None', 'Female Body Builders', 'fantasy'], ['None', 'FightingCatfight', 'force', ' group'], ['None', 'gorean', 'slave'], ['None', 'Infantilism', 'ABDL', ' role', ' WS'], ['None', 'Leathermanwoman', 'leather'], ['None', 'masks', 'clothing'], ['None', 'Medical Play', 'medical'], ['None', 'Personality Modification', 'role', ' ext'], ['None', 'Phone Sex commercial provider', 'talk'], ['None', 'Phone Sex serving Dom', 'None'], ['None', 'Phone Sex serving Dom’s friends', 'talk', ' sub', ' oth'], ['None', 'Prison Scenes', 'scene', ' fant'], ['None', 'Prostitution fantasy', 'force', ' fant'], ['None', 'Punishment Scene', 'pain', ' fant'], ['None', 'Religious Scenes', 'fantasy'], ['None', 'Serving as MaidButler', 'serve', ' role'], ['None', 'Serving as WaitressWaiter', 'role'], [' Toy', ' strap-on', 'Penetration', ' vaginal', 'strap'], ['medical', 'Speculums vaginal', 'speculum'], ['None', 'Sexual Addiction', 'fantasy'], ['None', 'Sexual intercourse with older person Gerontophilia', 'age'], ['None', 'sissification', 'orientation'], ['None', 'sleep', 'activity'], ['None', 'smoking', 'smoke'], ['None', 'tranny', 'orientation'], ['None', 'Vore eating pplthings', 'fetish']], 'Groups': [['anonymous', 'strangers', 'group', ' orgy', ' anon'], ['Gentle', 'Flirting', 'gentle', ' talk', ' group'], ['Orgy', 'Orgy', ' general', 'orgy'], ['Orgy', 'Gangbangs', 'force', ' group', ' role'], ['Orgy', 'Glory Hole', 'anon', ' orgy', ' cover'], ['orgy', 'Swapping hard', 'swap', ' group'], ['orgy', 'Triple Penetration multiple partners', 'orgy', ' triple', ' TP'], ['swing', 'Swapping soft', 'swap', ' group'], ['swing', 'Swinging', 'swing'], ['threesome', 'threesomes', '3']], 'WatchingShowing': [[' Dance', ' watch', 'Belly Dance', 'None'], ['Dance', ' watch', 'Erotic', 'None'], ['Media', ' Amateur', 'Take pictures', ' artistic', 'None'], ['Media', ' Amateur', 'Take pictures', ' dirty', 'None'], ['Media', ' Amateur', 'View pictures', ' artistic', 'None'], ['Media', ' Amateur', 'Receiving', 'None'], ['Media', ' Amateur', 'Video Taped Scenes', 'None'], ['Media', ' Amateur', 'Watch movie', 'None'], ['Media', ' Erotica', 'Read', 'None'], ['Public', ' indoors', 'Swingers Club', ' no interaction', 'None'], ['Public', ' show', 'Public Sex', ' rural', 'None'], ['Public', ' show', 'Strangers', ' individual or small', 'None'], ['Public', ' show', 'Known', 'None'], ['Public', ' show', 'Public Sex', 'None'], ['Public', ' show', 'Slutty Clothing public', 'None'], ['Public', ' sneak', 'Outdoor Sceneing', 'None'], ['Public', ' sneak', 'Secret Sex in Public', 'None'], ['Public', ' touch', 'Masturbation', ' known', 'None'], ['Public', ' touch', 'Masturbation', ' Partner', 'None'], ['Public', ' touch', 'Masturbation', ' strangers', 'None'], ['None', 'Submission public', 'None'], ['None', 'Video PornBDSM watching', 'None'], ['None', 'Voyeurism watching partner with others', 'None'], ['None', 'voyeurism', 'None'], ['Public', 'Wear collar', 'None']], 'Sensations': [['Oral sex', ' surprise', 'Being woken up with oral sex', 'surprise', ' sex', ' oral'], ['Toy', ' enema', 'Enema for punishment', 'enema'], ['Toys', ' electroc', 'TENS unit electrical toy', 'TENS'], ['dirt', 'Dirty Sex mud', ' dirt', ' oil', ' rubbish', 'dirty'], ['dom', 'Hair Holding', 'dom', ' hair'], ['hand jobs', 'Receiving', 'HJ'], ['nearsex', 'Frottage arousal from the rubbing of clothed bodies', 'nearsex'], ['None', 'Auto-Erotic Asphyxiation to heighten sexual pleasure', 'breath'], ['None', 'Bareback sexual intercourse without using a condom', 'condom'], ['None', 'Ben Wa Balls', 'toy', ' ben wa'], ['None', 'Choking', 'None'], ['None', 'Douching', 'toy', ' med'], ['None', 'Edge Play extending the boundaries of play', 'orgasm control'], ['None', 'Femoral Intercourse fucking closed thighs', 'nearsex'], ['None', 'Genital Sex', 'None'], ['None', 'Ice Cubes', 'cold'], ['None', 'impact play', 'impact'], ['None', 'Lifting and Carrying cradle style in arms', 'cuddle'], ['None', 'massages', 'letters'], ['None', 'Multiple Orgasms', 'orgasm'], ['None', 'Nipple Sucking', 'nipple'], ['None', 'Practice Save Sex', 'condom'], ['None', 'Pumping', 'pumping'], ['None', 'Skinny Dipping', 'nudity', ' water'], ['None', 'Temperature Play', 'hotcold'], ['None', 'The Zipper', 'None'], ['None', 'Triple Penetration dildo', 'toy', ' triple'], ['None', 'Urethral Sounds metal rods', 'toy', ' med'], ['None', 'Vibrator external', 'toy'], ['None', 'Vibrator internal', 'toy'], ['Pain', 'Nipple Play', ' painful', 'nipple'], ['toy', 'hot oil', 'toy', ' oil'], ['Toy', 'Inflatable Toys', 'toy', ' ext'], ['vagina', 'G-spot', 'pussy'], ['vaginal fisting', 'Giving', 'fisting']], 'BodyParts': [['Genitalia', ' Female', 'Pussy pumping', 'toy', ' pump'], ['Genitalia', ' Female', 'touch', 'labia'], ['Genitalia', ' Female', 'Worship', 'slave', ' serve', ' femdom', ' pussy'], ['Modify', ' extreme', 'Plastic Surgery', 'plastic surgery', ' watch'], ['Tattoo', ' temporary', 'Receiving', 'modify', ' tattoo', ' body'], ['Breast', 'BreastChest Fucking', 'titty fuck', ' breast', ' sex', ' frot'], ['Foot', 'Touch', ' during intercourse', 'foot', ' fetish', ' sex'], ['Foot', 'Touch', ' not intercourse', 'foot', ' standalone'], ['Foot', 'Toe Sucking', 'toe', ' foot'], ['Foot', 'View', 'foot', ' watch'], ['Genitals', 'Urethral Play', 'sounding', ' catheter'], ['hair', 'Shaving', ' non-genitals', 'shave', ' hair'], ['Hair', 'body hair', 'bush'], ['Hair', 'Shaved Genitals having', 'repeat', ' non genita;'], ['Hair', 'Shaved Genitals on partner', 'hair', ' shave', ' partner'], ['hand', 'Hand Licking', 'lick', ' hand'], ['Hands', 'Giving', 'handjob', ' hand'], ['modify', 'Anthropomorphic self contrived or false amputation', 'extreme', ' amputate'], ['Mouth', 'tongues', 'tongue'], ['Nipples', 'Touch', ' manual', 'nipple', ' breast', ' rub nipple'], ['Nipples', 'Touch', ' oral', 'nipple', ' breast', ' lick'], ['Other', 'armpit hair', 'hair', ' armpit', ' natural'], ['Other', 'Finger Sucking', 'finger', ' suck'], ['Penis', 'Cock Worship', 'penis', ' fetish'], ['urethra', 'urethral play', 'urethra', ' sounding', ' medical', ' catheter']]}


    def build_dict(corpus, stopwords=stopwords, measure='IDF'):
        '''
        @param corpus:    a list of documents, represented as lists of (stemmed)
                          words
        @param stopwords: the list of (stemmed) words that should have zero weight
        @param measure:   the measure used to compute the weights ('IDF'
                          i.e. 'inverse document frequency' or 'ICF' i.e.
                          'inverse collection frequency'; defaults to 'IDF')
        @returns: a dictionary of weights in the interval [0,1]
        '''
    
        import collections
        import math
    
        dictionary = {}
    
        if measure == 'ICF':
            words = [w for doc in corpus for w in doc]
    
            term_count = collections.Counter(words)
            total_count = len(words)
            scale = math.log(total_count)
    
            for w, cnt in term_count.iteritems():
                dictionary[w] = math.log(total_count / (cnt + 1)) / scale
    
        elif measure == 'IDF':
            corpus_size = len(corpus)
            scale = math.log(corpus_size)
    
            term_count = collections.defaultdict(int)
    
            for doc in corpus:
                words = set(doc)
                for w in words:
                    term_count[w] += 1
    
            for w, cnt in term_count.iteritems():
                dictionary[w] = math.log(corpus_size / (cnt + 1)) / scale
    
        if stopwords:
            for w in stopwords:
                dictionary[w] = 0.0
    
        return dictionary


    import operator
    from operator import *
    from nltk.corpus import wordnet as wn
    _hyp = operator.methodcaller("hypernyms")
    _i_hyp = operator.methodcaller("instance_hypernyms")
    _hypo = operator.methodcaller("hyponyms")
    _i_hypo = operator.methodcaller("instance_hyponyms")
    _m_holo = operator.methodcaller("member_holonyms")
    _d = operator.methodcaller("definition")
    _ex = operator.methodcaller("examples")
    _s_holo = operator.methodcaller("substance_holonyms")
    _p_holo = operator.methodcaller("part_holonyms")
    _m_mero = operator.methodcaller("member_meronyms")
    _s_mero = operator.methodcaller("substance_meronyms")
    _p_mero = operator.methodcaller("part_meronyms")
    _attrs = operator.methodcaller("attributes")
    _ents = operator.methodcaller("entailments")
    _causes = operator.methodcaller("causes")
    _also_sees = operator.methodcaller("also_sees")
    _verb_groups = operator.methodcaller("verb_groups")
    _similar_tos = operator.methodcaller("similar_tos")
    _lemma_names = operator.methodcaller("lemma_names")
    Synset = wn.synset
    def SMV():
    	text = input('text')
    	dic = {}
    	words = wn.synsets(text)
    	method_names = ['hypernyms', 'instance_hypernyms', 'hyponyms', 'instance_hyponyms', 'member_holonyms', 'definition', 'examples', 'substance_holonyms', 'part_holonyms', 'member_meronyms', 'substance_meronyms', 'part_meronyms','attributes', 'entailments', 'causes', 'also_sees', 'verb_groups', 'similar_tos', 'lemma_names']
    	for word in words:
    		dic[word] = {}
    		synset_title = ''.join(word._name.split('.'))
    		for method_name in method_names:
    			method = getattr(wn.synset(word._name), method_name)
    			vals = method()
    			if vals == []:
    				pass
    			else:
    				dic[word][method_name] = vals
    	return dic
    
    import sympy
    from sympy import *
    SMV()
    #penis = wn.synset('penis.n.01')
    
    
    #dic = {'hypernyms':[p.name()for p in penis.hypernyms()],
    #'instance_hypernyms': [p.name()for p in penis.instance_hypernyms()],
    #'hyponyms': [p.name()for p in penis.hyponyms()],
    #'instance_hyponyms': [p.name()for p in penis.instance_hyponyms()],
    #'member_holonyms':[p.name()for p in penis.member_holonyms()],
    #'substance_holonyms':[p.name()for p in penis.substance_holonyms()],
    #'part_holonyms':[p.name()for p in penis.part_holonyms()],
    #'substance_meronyms':[p.name()for p in penis.substance_meronyms()],
    #'part_meronyms':[p.name()for p in penis.part_meronyms()],
    #'member_meronyms':[p.name()for p in penis.member_meronyms()],
    #'lemma_names' : penis.lemma_names(),
    #'part_meronyms': [p.name()for p in penis.part_meronyms()]}



    import operator
    from operator import *



    
    r =['hypernyms', 'instance_hypernyms', 'hyponyms', 'instance_hyponyms', 'member_holonyms', 'definition', 'examples', 'substance_holonyms', 'part_holonyms', 'member_meronyms', 'substance_meronyms', 'part_meronyms','attributes', 'entailments', 'causes', 'also_sees', 'verb_groups', 'similar_tos', 'lemma_names']
    
    sex = wn.synset('sex.n.01')
    _hyp = operator.methodcaller("hypernyms")
    _i_hyp = operator.methodcaller("instance_hypernyms")
    _hypo = operator.methodcaller("hyponyms")
    _i_hypo = operator.methodcaller("instance_hyponyms")
    _m_holo = operator.methodcaller("member_holonyms")
    _d = operator.methodcaller("definition")
    _ex = operator.methodcaller("examples")
    _s_holo = operator.methodcaller("substance_holonyms")
    _p_holo = operator.methodcaller("part_holonyms")
    _m_mero = operator.methodcaller("member_meronyms")
    _s_mero = operator.methodcaller("substance_meronyms")
    _p_mero = operator.methodcaller("part_meronyms")
    _attrs = operator.methodcaller("attributes")
    _ents = operator.methodcaller("entailments")
    _causes = operator.methodcaller("causes")
    _also_sees = operator.methodcaller("also_sees")
    _verb_groups = operator.methodcaller("verb_groups")
    _similar_tos = operator.methodcaller("similar_tos")
    _lemma_names = operator.methodcaller("lemma_names")
    [h.name() for h in _hypo(sex)]


    


    


    


    
