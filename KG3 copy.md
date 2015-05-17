
#KGerring Basic Notes*
Updates:  May 17, 2015



    import itertools, concepts, tablib
    from tablib import *
    from itertools import *
    from pprint import pprint as pprint



    source= ( '''
                  |length|change|preference|basis    |gender   |choice |
    Heterosexual  | one  |static|strong    |thoughts |opposite |gender |
    Homosexual    | one  |static|strong    |thoughts |same     |gender |
    Bisexual      | two  |static|none      |thoughts |opposite |gender |
    Heteroflexible| two  |fluid |mild      |acts     |opposite |novelty|
    Homoflexible  | two  |fluid |mild      |acts     |same     |novelty|
    Pansexual     |  0   |static|none      |thoughts |opposite |other  |
    In_flux       |  0   |fluid |other     |thoughts |opposite |unsure |
    Asexual       | zero |static|strong    |thoughts |opposite |other  |
    Unsure_NA     | NA   |fluid |other     |acts     |opposite |other  |
    ''')
    
    class Source:
        """Source Stuff"""
        def __init__(self, name, source):
            self.name=name
            self.source=s
    
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
            return "Source:" +str(self.__dict__)`
    
    
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
      from IPython.display import Image
    Image(filename='/Users/Kristen/figure_1.png')




![png](output_3_0.png)



######Old Notes to update   
    
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
    Objects = ['Heterosexual', 'Homosexual', 'Bisexual', 'Heteroflexible', 'Homoflexible', 'Pansexual', 'In_flux', 'Asexual']



  

    import concepts
    from concepts import *
    sexual_orientation= Context.fromstring('''
                  |0-1|1+  |Static|Fluid|Biased|Neutral|Thought|Act|~same|~opp|Gendered|Othered|
    Heterosexual  | x |    |  x   |     |   x  |       |   x   |   |     |   x|   x    |       |
    Homosexual    | x |    |  x   |     |   x  |       |   x   |   |    x|    |   x    |       |
    Bisexual      |   |   x|  x   |     |      |     x |   x   |   |     |    |   x    |       |
    Heteroflexible|   |   x|      |   x |   x  |       |       |  x|     |   x|   x    |       |
    Homoflexible  |   |   x|      |   x |   x  |       |       |  x|    x|    |   x    |       |
    Queer         | x |  x |      |   x |   x  |     x |   x   |   |     |    |   x    |    x  |
    Pansexual     |   |   x|  x   |     |      |     x |   x   |   |     |    |        |    x  |
    in_flux       | x |   x|      |   x |   x  |       |   x   |   |     |    |   x    |       |
    Asexual       | x |   x|  x   |     |      |     x |   x   |   |     |    |        |    x  |
    Unsure_NA     |   |   x|      |    x|      |       |       |   |     |    |        |    x  |
     ''')
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
    print(d)

    [['Heterosexual', '0-1'], ['Heterosexual', '1+'], ['Heterosexual', 'Static'], ['Heterosexual', 'Fluid'], ['Heterosexual', 'Biased'], ['Heterosexual', 'Neutral'], ['Heterosexual', 'Thought'], ['Heterosexual', 'Act'], ['Heterosexual', '~same'], ['Heterosexual', '~opp'], ['Heterosexual', 'Gendered'], ['Heterosexual', 'Othered'], ['Homosexual', '0-1'], ['Homosexual', '1+'], ['Homosexual', 'Static'], ['Homosexual', 'Fluid'], ['Homosexual', 'Biased'], ['Homosexual', 'Neutral'], ['Homosexual', 'Thought'], ['Homosexual', 'Act'], ['Homosexual', '~same'], ['Homosexual', '~opp'], ['Homosexual', 'Gendered'], ['Homosexual', 'Othered'], ['Bisexual', '0-1'], ['Bisexual', '1+'], ['Bisexual', 'Static'], ['Bisexual', 'Fluid'], ['Bisexual', 'Biased'], ['Bisexual', 'Neutral'], ['Bisexual', 'Thought'], ['Bisexual', 'Act'], ['Bisexual', '~same'], ['Bisexual', '~opp'], ['Bisexual', 'Gendered'], ['Bisexual', 'Othered'], ['Heteroflexible', '0-1'], ['Heteroflexible', '1+'], ['Heteroflexible', 'Static'], ['Heteroflexible', 'Fluid'], ['Heteroflexible', 'Biased'], ['Heteroflexible', 'Neutral'], ['Heteroflexible', 'Thought'], ['Heteroflexible', 'Act'], ['Heteroflexible', '~same'], ['Heteroflexible', '~opp'], ['Heteroflexible', 'Gendered'], ['Heteroflexible', 'Othered'], ['Homoflexible', '0-1'], ['Homoflexible', '1+'], ['Homoflexible', 'Static'], ['Homoflexible', 'Fluid'], ['Homoflexible', 'Biased'], ['Homoflexible', 'Neutral'], ['Homoflexible', 'Thought'], ['Homoflexible', 'Act'], ['Homoflexible', '~same'], ['Homoflexible', '~opp'], ['Homoflexible', 'Gendered'], ['Homoflexible', 'Othered'], ['Queer', '0-1'], ['Queer', '1+'], ['Queer', 'Static'], ['Queer', 'Fluid'], ['Queer', 'Biased'], ['Queer', 'Neutral'], ['Queer', 'Thought'], ['Queer', 'Act'], ['Queer', '~same'], ['Queer', '~opp'], ['Queer', 'Gendered'], ['Queer', 'Othered'], ['Pansexual', '0-1'], ['Pansexual', '1+'], ['Pansexual', 'Static'], ['Pansexual', 'Fluid'], ['Pansexual', 'Biased'], ['Pansexual', 'Neutral'], ['Pansexual', 'Thought'], ['Pansexual', 'Act'], ['Pansexual', '~same'], ['Pansexual', '~opp'], ['Pansexual', 'Gendered'], ['Pansexual', 'Othered'], ['in_flux', '0-1'], ['in_flux', '1+'], ['in_flux', 'Static'], ['in_flux', 'Fluid'], ['in_flux', 'Biased'], ['in_flux', 'Neutral'], ['in_flux', 'Thought'], ['in_flux', 'Act'], ['in_flux', '~same'], ['in_flux', '~opp'], ['in_flux', 'Gendered'], ['in_flux', 'Othered'], ['Asexual', '0-1'], ['Asexual', '1+'], ['Asexual', 'Static'], ['Asexual', 'Fluid'], ['Asexual', 'Biased'], ['Asexual', 'Neutral'], ['Asexual', 'Thought'], ['Asexual', 'Act'], ['Asexual', '~same'], ['Asexual', '~opp'], ['Asexual', 'Gendered'], ['Asexual', 'Othered'], ['Unsure_NA', '0-1'], ['Unsure_NA', '1+'], ['Unsure_NA', 'Static'], ['Unsure_NA', 'Fluid'], ['Unsure_NA', 'Biased'], ['Unsure_NA', 'Neutral'], ['Unsure_NA', 'Thought'], ['Unsure_NA', 'Act'], ['Unsure_NA', '~same'], ['Unsure_NA', '~opp'], ['Unsure_NA', 'Gendered'], ['Unsure_NA', 'Othered']]


###DiGraph Concept Lattice with nodes mapped w/**Fruchterman-Reingold** force-directed algorithm.


    from IPython.display import Image
    Image(filename='/Users/Kristen/Lattice1.png') 
    Image(filename='/Users/Kristen/Homoflexible.png') 




![png](output_6_0.png)




    Galen Properties: To_Do


    thing =['thing']
    top_category =['top_category', 'thing' ],
    domain_category =['domain_category']
    generalised_structure =['generalised_structure', 'domain_category']
    abstract_structure =['abstract_structure', 'generalised_structure', 'domain_category']
    logical_structure =['logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    frame_of_reference =['frame_of_reference', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    plane_of_reference =['plane_of_reference', 'frame_of_reference', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    point_of_reference =['point_of_reference', 'frame_of_reference', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    reference_structure =['reference_structure', 'frame_of_reference', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    ideas =['ideas', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    plan =['plan', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    result =['result', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    schema =['schema', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    signal =['signal', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    speech =['speech', 'signal', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    system =['system', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    anatomical_system =['anatomical_system', 'system', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    digestive_system =['digestive_system', 'anatomical_system', 'system', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    genito_urinary_system =['genito_urinary_system', 'anatomical_system', 'system', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    female_genito_urinary_system =['female_genito_urinary_system', 'genito_urinary_system', 'anatomical_system', 'system', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    male_genito_urinary_system =['male_genito_urinary_system', 'genito_urinary_system', 'anatomical_system', 'system', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    functional_system =['functional_system', 'system', 'logical_structure', 'abstract_structure', 'generalised_structure', 'domain_category']
    psychosocial_construct =['psychosocial_construct', 'abstract_structure', 'generalised_structure', 'domain_category']
    psychological_construct =['psychological_construct', 'psychosocial_construct', 'abstract_structure', 'generalised_structure', 'domain_category']
    faith =['faith', 'psychological_construct', 'psychosocial_construct', 'abstract_structure', 'generalised_structure', 'domain_category']
    memory =['memory', 'psychological_construct', 'psychosocial_construct', 'abstract_structure', 'generalised_structure', 'domain_category']
    social_organisation =['social_organisation', 'psychosocial_construct', 'abstract_structure', 'generalised_structure', 'domain_category']
    physical_structure =['physical_structure', 'generalised_structure', 'domain_category']
    linear_structure =['linear_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    displacement =['displacement', 'linear_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    solid_structure =['solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    body_structure =['body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    abnormal_body_structure =['abnormal_body_structure', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    body_part =['body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_internal_body_part =['named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_genital_tract_body_part =['named_genital_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_female_genital_tract_body_part =['named_female_genital_tract_body_part', 'named_genital_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    uterus =['uterus', 'named_female_genital_tract_body_part', 'named_genital_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    vagina =['vagina', 'named_female_genital_tract_body_part', 'named_genital_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_male_genital_tract_body_part =['named_male_genital_tract_body_part', 'named_genital_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    testis =['testis', 'named_female_genital_tract_body_part', 'named_genital_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_g_i_tract_body_part =['named_g_i_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    anal_canal =['anal_canal', 'named_g_i_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    colon =['colon', 'named_g_i_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    descending_colon =['descending_colon', 'named_g_i_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    intestine =['intestine', 'named_g_i_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    intestine_or_stomach =['intestine_or_stomach', 'named_g_i_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    rectum =['rectum', 'named_g_i_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    rectum_or_colon =['rectum_or_colon', 'named_g_i_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_nervous_system_part =['named_nervous_system_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    brain =['brain', 'named_nervous_system_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    cerebral_hemisphere =['cerebral_hemisphere', 'named_nervous_system_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_sensory_part =['named_sensory_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    fundus_oculi =['fundus_oculi', 'named_sensory_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    inner_ear =['inner_ear', 'named_sensory_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_urinary_tract_body_part =['named_urinary_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    internal_urethral_orifice =['internal_urethral_orifice', 'named_urinary_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    ureter =['ureter', 'named_urinary_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    urethra =['urethra', 'named_urinary_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    urinary_bladder =['urinary_bladder', 'named_urinary_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    urinary_tract =['urinary_tract', 'named_urinary_tract_body_part', 'named_internal_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_surface_body_part =['named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    body_junctional_body_part =['body_junctional_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    axilla =['axilla', 'body_junctional_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    buttock =['buttock', 'body_junctional_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    groin =['groin', 'body_junctional_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    hip =['hip', 'body_junctional_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    shoulder =['shoulder', 'body_junctional_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    extremity_body_part =['extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    digit =['digit', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    finger =['finger', 'digit', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    toe =['toe', 'digit', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    main_extremity_body_part =['main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    extremity_joint_part =['extremity_joint_part', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    ankle =['ankle','extremity_joint_part', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    elbow =['elbow','extremity_joint_part', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    knee =['knee','extremity_joint_part', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    wrist =['wrist','extremity_joint_part', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    extremity_long_part =['extremity_long_part', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    arm =['arm','extremity_long_part', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    forearm =['forearm','extremity_long_part', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    leg =['leg','extremity_long_part', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    thigh =['thigh','extremity_long_part', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    hand_or_foot =['hand_or_foot', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    foot =['foot','hand_or_foot', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    hand =['hand','hand_or_foot', 'main_extremity_body_part', 'extremity_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    major_body_division =['major_body_division', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    body_as_awhole =['body_as_awhole', 'major_body_division', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    extremity =['extremity', 'major_body_division', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    head =['head', 'major_body_division', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    head_and_neck =['head_and_neck', 'major_body_division', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    neck =['neck', 'major_body_division', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    trunk =['trunk', 'major_body_division', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_head_surface_body_part =['named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    cheek =['cheek', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    chin =['chin', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    ear =['ear', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    external_aspect_of_eye =['external_aspect_of_eye', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    face =['face', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    forehead =['forehead', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    jaw =['jaw', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    lip =['lip', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    mouth =['mouth', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    nose =['nose', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    scalp =['scalp', 'named_head_surface_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_trunk_body_part =['named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    abdomen =['abdomen', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    back =['back', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    breast =['breast', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    chest =['chest', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    flank =['flank', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    inguinal_region =['inguinal_region', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_genital_surface_body_part =['named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_female_genital_surface_body_part =['named_female_genital_surface_body_part', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    clitoris =['clitoris','named_female_genital_surface_body_part', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    female_external_genitalia =['female_external_genitalia','named_female_genital_surface_body_part', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    mons_pubis =['mons_pubis','named_female_genital_surface_body_part', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    vulva =['vulva','named_female_genital_surface_body_part', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    named_male_genital_surface_body_part =['named_male_genital_surface_body_part', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    male_external_genitalia =['male_external_genitalia','named_male_genital_surface_body_part', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    penis =['penis','named_male_genital_surface_body_part', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    scrotum =['scrotum','named_male_genital_surface_body_part', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    nipple =['nipple', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    pelvis =['pelvis', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    perineum =['perineum', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    pubes =['pubes', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    thorax =['thorax', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    umbilicus =['umbilicus', 'named_genital_surface_body_part', 'named_trunk_body_part', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    surface_body_landmark =['surface_body_landmark', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    bregma =['bregma', 'surface_body_landmark', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    linea_alba =['linea_alba', 'surface_body_landmark', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    mid_clavicular_line =['mid_clavicular_line', 'surface_body_landmark', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    scapular_line =['scapular_line', 'surface_body_landmark', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    waist =['waist', 'surface_body_landmark', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    surface_opening =['surface_opening', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    anus =['anus', 'surface_opening', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    external_auditory_meatus =['external_auditory_meatus', 'surface_opening', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    introitus =['introitus', 'surface_opening', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    mouth =['mouth', 'surface_opening', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    naris =['naris', 'surface_opening', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    urethral_meatus =['urethral_meatus', 'surface_opening', 'named_surface_body_part', 'body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    surface_configuration =['surface_configuration'' body_part', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    generic_body_structure =['generic_body_structure', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    generic_body_surface_structure =['generic_body_surface_structure', 'generic_body_structure', 'body_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    built_structure =['built_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    communication_structure =['communication_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    computer =['computer', 'communication_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    paper =['paper', 'communication_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    telephone =['telephone', 'communication_structure', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    device =['device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    animal_organ =['animal_organ', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    animal_tissue =['animal_tissue', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    connecting_material =['connecting_material', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    cutting_tool =['cutting_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    knife =['knife', 'cutting_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    scissors =['scissors', 'cutting_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    shaver =['shaver', 'cutting_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    fixation_device =['fixation_device', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    nail =['nail', 'fixation_device', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    pin =['pin', 'fixation_device', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    plate =['plate', 'fixation_device', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    rod =['rod', 'fixation_device', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    screw =['screw', 'fixation_device', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    holding_tool =['holding_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    laboratory_machine =['laboratory_machine', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    opening_tool =['opening_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    drill =['drill', 'opening_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    knife =['knife', 'opening_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    laser =['laser', 'opening_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    obturator =['obturator', 'opening_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    saw =['saw', 'opening_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    scissors =['scissors', 'opening_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    solid_needle =['solid_needle', 'opening_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    trocar =['trocar', 'opening_tool', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    probe =['probe', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    surgical_instrument =['surgical_instrument', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    transport_device =['transport_device', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    catheter =['catheter', 'transport_device', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    tube =['tube', 'transport_device', 'device', 'solid_structure', 'physical_structure', 'generalised_structure', 'domain_category']
    generalised_substance =['generalised_substance', 'domain_category']
    energy =['energy', 'generalised_substance', 'domain_category']
    heat =['heat', 'energy', 'generalised_substance', 'domain_category']
    light =['light', 'energy', 'generalised_substance', 'domain_category']
    sound =['sound', 'energy', 'generalised_substance', 'domain_category']
    substance =['substance', 'generalised_substance', 'domain_category']
    body_substance =['body_substance', 'substance', 'generalised_substance', 'domain_category']
    intrinsically_pathological_body_substance =['intrinsically_pathological_body_substance', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    named_body_substance =['named_body_substance', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    body_fluid =['body_fluid', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    bile =['bile', 'body_fluid', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    feces =['feces', 'body_fluid', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    gastric_acid =['gastric_acid', 'body_fluid', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    milk =['milk', 'body_fluid', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    mucus =['mucus', 'body_fluid', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    pus =['pus', 'body_fluid', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    saliva =['saliva', 'body_fluid', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    tears =['tears', 'body_fluid', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    urine =['urine', 'body_fluid', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    pathological_body_substance =['pathological_body_substance', 'body_substance', 'substance', 'generalised_substance', 'domain_category']
    chemical_substance =['chemical_substance', 'substance', 'generalised_substance', 'domain_category']
    modifier_concept =['modifier_concept', 'domain_category']
    aspect =['aspect', 'modifier_concept', 'domain_category']
    feature =['feature', 'aspect', 'modifier_concept', 'domain_category']
    organism_feature =['organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    age =['age', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    anaesthesia =['anaesthesia', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    body_position =['body_position', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    clinical_speciality =['clinical_speciality', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    dependency =['dependency', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    high_dependency =['high_dependency', 'dependency', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    independant =['independant', 'dependency', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    low_dependency =['low_dependency', 'dependency', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    medium_dependency =['medium_dependency', 'dependency', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    grade_of_experience =['grade_of_experience', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    gravidity =['gravidity', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    level_of_consciousness =['level_of_consciousness', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    agitated =['agitated', 'level_of_consciousness', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    coma =['coma', 'level_of_consciousness', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    normal_level_of_consciousness =['normal_level_of_consciousness', 'level_of_consciousness', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    reduced_level_of_consciousness =['reduced_level_of_consciousness', 'level_of_consciousness', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    mobility =['mobility', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    immobility =['immobility', 'mobility', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    increased_mobility =['increased_mobility', 'mobility', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    normal_mobility =['normal_mobility', 'mobility', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    reduced_mobility =['reduced_mobility', 'mobility', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    sex =['sex', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    viability =['viability', 'organism_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    process_feature =['process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    completeness =['completeness', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    functional_feature =['functional_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    process_activity =['process_activity', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    process_activity_level =['process_activity_level', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    decreased_activity_level =['decreased_activity_level', 'process_activity_level', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    high_activity_level =['high_activity_level', 'process_activity_level', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    increased_activity_level =['increased_activity_level', 'process_activity_level', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    low_activity_level =['low_activity_level', 'process_activity_level', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    range =['range', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    scope =['scope', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    sensitivity =['sensitivity', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    resistance =['resistance', 'sensitivity', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    sensitivity =['sensitivity', 'sensitivity', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    temporal_feature =['temporal_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    chronicity =['chronicity', 'temporal_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    duration =['duration', 'temporal_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    finish_time =['finish_time', 'temporal_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    frequency =['frequency', 'temporal_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    process_pattern =['process_pattern', 'temporal_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    start_time =['start_time', 'temporal_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    time_of_occurrence =['time_of_occurrence', 'temporal_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    volitional_feature =['volitional_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    urgency =['urgency', 'volitional_feature', 'process_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    quantity_feature =['quantity_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    proportion =['proportion', 'quantity_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    structural_feature =['structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    consistency =['consistency', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    dimension =['dimension', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    absolute_measurement =['absolute_measurement', 'dimension', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    area =['area', 'absolute_measurement', 'dimension', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    length =['length', 'absolute_measurement', 'dimension', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    level =['level', 'absolute_measurement', 'dimension', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    mass =['mass', 'absolute_measurement', 'dimension', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    speed =['speed', 'absolute_measurement', 'dimension', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    volume =['volume', 'absolute_measurement', 'dimension', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    relative_measurement =['relative_measurement', 'dimension', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    quality =['quality', 'relative_measurement', 'dimension', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    morphology =['morphology', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    appearance =['appearance', 'morphology', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    colour =['colour', 'morphology', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    shape =['shape', 'morphology', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    sharpness =['sharpness', 'morphology', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    size =['size', 'morphology', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    texture =['texture', 'morphology', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    position =['position', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    anterior_posterior_position =['anterior_posterior_position', 'position', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    lateral_position =['lateral_position', 'position', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    medial_lateral_position =['medial_lateral_position', 'position', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    proximal_distal_position =['proximal_distal_position', 'position', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    superior_inferior_position =['superior_inferior_position', 'position', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    upper_lower_position =['upper_lower_position', 'position', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    visibility =['visibility', 'structural_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    substance_feature =['substance_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    chemical_feature =['chemical_feature', 'substance_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    concentration =['concentration', 'substance_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    physical_feature =['physical_feature', 'substance_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    pressure =['pressure', 'physical_feature', 'substance_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    temperature =['temperature', 'physical_feature', 'substance_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    physical_state =['physical_state', 'substance_feature', 'feature', 'aspect', 'modifier_concept', 'domain_category']
    state =['state', 'aspect', 'modifier_concept', 'domain_category']
    abstract_state =['abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    level_state =['level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    absolute_level_state =['absolute_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    high_level =['high_level', 'absolute_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    low_level =['low_level', 'absolute_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    moderate_level =['moderate_level', 'absolute_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    undetected_level =['undetected_level', 'absolute_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    change_in_level_state =['change_in_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    decreased =['decreased', 'change_in_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    increased =['increased', 'change_in_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    unchanged =['unchanged', 'change_in_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    level_expectation_state =['level_expectation_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    depressed_level =['depressed_level', 'level_expectation_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    elevated_level =['elevated_level', 'level_expectation_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    expected_level =['expected_level', 'level_expectation_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    unexpected_level =['unexpected_level', 'level_expectation_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    depressed_level =['depressed_level', 'unexpected_level', 'level_expectation_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    elevated_level =['elevated_level', 'unexpected_level', 'level_expectation_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    trend_in_level_state =['trend_in_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    decreasing =['decreasing', 'trend_in_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    increasing =['increasing', 'trend_in_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    stable =['stable', 'trend_in_level_state', 'level_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    positive_negative_state =['positive_negative_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    equivocal =['equivocal', 'positive_negative_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    negative =['negative', 'positive_negative_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    positive =['positive', 'positive_negative_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    quality_state =['quality_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    average_quality =['average_quality', 'quality_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    bad_quality =['bad_quality', 'quality_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    excellent_quality =['excellent_quality', 'quality_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    good_quality =['good_quality', 'quality_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    poorquality =['poorquality', 'quality_state', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    quantity =['quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    numeric_quantity =['numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    acceleration_value =['acceleration_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    area_value =['area_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    concentration_value =['concentration_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    count_concentration_value =['count_concentration_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    distance_value =['distance_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    flow_rate_value =['flow_rate_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    frequency_value =['frequency_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    mass_value =['mass_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    pressure_value =['pressure_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    speed_value =['speed_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    temperature_value =['temperature_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    temporal_value =['temporal_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    volume_value =['volume_value', 'numeric_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    ordinal_quantity =['ordinal_quantity', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    ratio =['ratio', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    a_part =['a_part', 'ratio', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    none =['none', 'ratio', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    the_whole =['the_whole', 'ratio', 'quantity', 'abstract_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    organism_state =['organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    age_state =['age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    adult =['adult', 'age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    middle_aged_person =['middle_aged_person', 'adult', 'age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    pensioner =['pensioner', 'adult', 'age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    young_adult =['young_adult', 'adult', 'age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    child =['child', 'age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    baby =['baby', 'child', 'age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    infant =['infant', 'baby', 'child', 'age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    neonate =['neonate', 'baby', 'child', 'age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    youth =['youth', 'age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    teenager =['teenager', 'child', 'age_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    body_position_state =['body_position_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    grade_of_experience_state =['grade_of_experience_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    consultant =['consultant', 'grade_of_experience_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    junior =['junior', 'grade_of_experience_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    senior =['senior', 'grade_of_experience_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    life_or_death_state =['life_or_death_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    death =['death', 'life_or_death_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    life =['life', 'life_or_death_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    pregnancy_state =['pregnancy_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    gravid =['gravid', 'pregnancy_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    non_gravid =['non_gravid', 'pregnancy_state', 'organism_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    process_state =['process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    activity_state =['activity_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    active =['active', 'activity_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    inactive =['inactive', 'activity_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    completeness_state =['completeness_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    partial =['partial', 'completeness_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    total =['total', 'completeness_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    direction_state =['direction_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    forward =['forward', 'direction_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    reverse =['reverse', 'direction_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    disease_state =['disease_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    severity_state =['severity_state', 'disease_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    mild =['mild', 'severity_state', 'disease_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    moderate =['moderate', 'severity_state', 'disease_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    severe =['severe', 'severity_state', 'disease_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    effectiveness_state =['effectiveness_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    effective =['effective', 'effectiveness_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    ineffective =['ineffective', 'effectiveness_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    sensitivity_state =['sensitivity_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    resistant =['resistant', 'sensitivity_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    sensitive =['sensitive', 'sensitivity_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    temporal_state =['temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    duration_state =['duration_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    long_time =['long_time', 'duration_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    short_time =['short_time', 'duration_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    frequency_state =['frequency_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    always =['always', 'frequency_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    never =['never', 'frequency_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    often =['often', 'frequency_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    sometimes =['sometimes', 'frequency_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    usually =['usually', 'frequency_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    process_pattern_state =['process_pattern_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    continuous =['continuous', 'process_pattern_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    intermittent =['intermittent', 'process_pattern_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    persistent =['persistent', 'process_pattern_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    single =['single', 'process_pattern_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    temporal_position_state =['temporal_position_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    future =['future', 'temporal_position_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    now =['now', 'temporal_position_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    past =['past', 'temporal_position_state', 'temporal_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    volitional_state =['volitional_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    urgency_state =['urgency_state', 'volitional_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    non_urgent =['non_urgent', 'urgency_state', 'volitional_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    elective =['elective', 'non_urgent', 'urgency_state', 'volitional_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    urgent =['urgent', 'urgency_state', 'volitional_state', 'process_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    structural_state =['structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    consistency_state =['consistency_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    firmness =['firmness', 'consistency_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    rigidity =['rigidity', 'consistency_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    softness =['softness', 'consistency_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    morphology_state =['morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    appearance_state =['appearance_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    colour_state =['colour_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    shape_state =['shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    change_in_shape_state =['change_in_shape_state', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    contracted =['contracted', 'change_in_shape_state', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    dilated =['dilated', 'change_in_shape_state', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    sharpness_state =['sharpness_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    blunt =['blunt', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    sharp =['sharp', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    size_state =['size_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    absolute_size_state =['absolute_size_state', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    large =['large', 'absolute_size_state', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    small =['small', 'absolute_size_state', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    change_in_size_state =['change_in_size_state', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    larger =['larger', 'change_in_size_state', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    smaller =['smaller', 'change_in_size_state', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    texture_state =['texture_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    rough =['rough', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    smooth =['smooth', 'shape_state', 'morphology_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    position_state =['position_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    absolute_position_state =['absolute_position_state', 'position_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    change_in_position_state =['change_in_position_state', 'position_state', 'structural_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    substance_state =['substance_state', 'state', 'aspect', 'modifier_concept', 'domain_category']
    status =['status', 'aspect', 'modifier_concept', 'domain_category']
    abstract_status =['abstract_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    countability_status =['countability_status', 'abstract_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    paired_or_unpaired_status =['paired_or_unpaired_status', 'abstract_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    at_least_paired =['at_least_paired', 'paired_or_unpaired_status', 'abstract_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    unpaired =['unpaired', 'paired_or_unpaired_status', 'abstract_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    organism_status =['organism_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    sex_status =['sex_status', 'organism_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    female =['female', 'sex_status', 'organism_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    male =['male', 'sex_status', 'organism_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    process_status =['process_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    structural_status =['structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    medical_status =['medical_status', 'structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    normal_or_non_normal_status =['normal_or_non_normal_status', 'medical_status', 'structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    non_normal =['non_normal', 'normal_or_non_normal_status', 'medical_status', 'structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    abnormal =['abnormal', 'normal_or_non_normal_status', 'medical_status', 'structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    unusual =['unusual', 'normal_or_non_normal_status', 'medical_status', 'structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    normal =['normal', 'normal_or_non_normal_status', 'medical_status', 'structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    pathological_or_physiological_status =['pathological_or_physiological_status', 'medical_status', 'structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    pathological =['pathological', 'pathological_or_physiological_status', 'medical_status', 'structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    physiological =['physiological', 'pathological_or_physiological_status', 'medical_status', 'structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    visibility_status =['visibility_status', 'structural_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    substance_status =['substance_status', 'status', 'aspect', 'modifier_concept', 'domain_category']
    general_level_of_specification =['general_level_of_specification', 'modifier_concept', 'domain_category']
    level_of_specification =['level_of_specification', 'general_level_of_specification', 'modifier_concept', 'domain_category']
    at_least_partially_specified =['at_least_partially_specified', 'level_of_specification', 'general_level_of_specification', 'modifier_concept', 'domain_category']
    at_leastwell_specified =['at_leastwell_specified', 'at_least_partially_specified', 'level_of_specification', 'general_level_of_specification', 'modifier_concept', 'domain_category']
    uniquely_specified =['uniquely_specified', 'at_leastwell_specified', 'at_least_partially_specified', 'level_of_specification', 'general_level_of_specification', 'modifier_concept', 'domain_category']
    process_level_of_specification =['process_level_of_specification', 'general_level_of_specification', 'modifier_concept', 'domain_category']
    at_least_partially_specified_process =['at_least_partially_specified_process', 'process_level_of_specification', 'general_level_of_specification', 'modifier_concept', 'domain_category']
    at_leastwell_specified_process =['at_leastwell_specified_process', 'at_least_partially_specified_process', 'process_level_of_specification', 'general_level_of_specification', 'modifier_concept', 'domain_category']
    modality =['modality', 'modifier_concept', 'domain_category']
    existentiality =['existentiality', 'modality', 'modifier_concept', 'domain_category']
    absence =['absence', 'existentiality', 'modality', 'modifier_concept', 'domain_category']
    presence =['presence', 'existentiality', 'modality', 'modifier_concept', 'domain_category']
    family_history =['family_history', 'modality', 'modifier_concept', 'domain_category']
    role =['role', 'modifier_concept', 'domain_category']
    physiological_role =['physiological_role', 'role', 'modifier_concept', 'domain_category']
    social_role =['social_role', 'role', 'modifier_concept', 'domain_category']
    slave_role =['slave_role', 'social_role', 'role', 'modifier_concept', 'domain_category']
    dominant_role =['dominant_role', 'social_role', 'role', 'modifier_concept', 'domain_category']
    master_role =['master_role', 'social_role', 'role', 'modifier_concept', 'domain_category']
    object_role =['object_role', 'social_role', 'role', 'modifier_concept', 'domain_category']
    submissive_role =['submissive_role', 'social_role', 'role', 'modifier_concept', 'domain_category']
    user_role =['user_role', 'social_role', 'role', 'modifier_concept', 'domain_category']
    unit =['unit', 'modifier_concept', 'domain_category']
    composite_unit =['composite_unit', 'unit', 'modifier_concept', 'domain_category']
    area_unit =['area_unit', 'composite_unit', 'unit', 'modifier_concept', 'domain_category']
    concentration_unit =['concentration_unit', 'composite_unit', 'unit', 'modifier_concept', 'domain_category']
    count_concentration_unit =['count_concentration_unit', 'composite_unit', 'unit', 'modifier_concept', 'domain_category']
    dose_unit =['dose_unit', 'composite_unit', 'unit', 'modifier_concept', 'domain_category']
    flow_rate_unit =['flow_rate_unit', 'composite_unit', 'unit', 'modifier_concept', 'domain_category']
    frequency_unit =['frequency_unit', 'composite_unit', 'unit', 'modifier_concept', 'domain_category']
    pressure_unit =['pressure_unit', 'composite_unit', 'unit', 'modifier_concept', 'domain_category']
    speed_unit =['speed_unit', 'composite_unit', 'unit', 'modifier_concept', 'domain_category']
    volume_unit =['volume_unit', 'composite_unit', 'unit', 'modifier_concept', 'domain_category']
    organism =['organism', 'domain_category']
    animal =['animal', 'organism', 'domain_category']
    process =['process', 'domain_category']
    behaviour =['behaviour', 'process', 'domain_category']
    pattern_of_behaviour =['pattern_of_behaviour', 'behaviour', 'process', 'domain_category']
    avoidance =['avoidance', 'pattern_of_behaviour', 'behaviour', 'process', 'domain_category']
    practice =['practice', 'pattern_of_behaviour', 'behaviour', 'process', 'domain_category']
    reflex =['reflex', 'behaviour', 'process', 'domain_category']
    volitional_act =['volitional_act', 'behaviour', 'process', 'domain_category']
    physiological_volitional_act =['physiological_volitional_act', 'volitional_act', 'behaviour', 'process', 'domain_category']
    voluntary_movement =['voluntary_movement', 'physiological_volitional_act', 'volitional_act', 'behaviour', 'process', 'domain_category']
    body_process =['body_process', 'process', 'domain_category']
    generic_control_process =['generic_control_process', 'body_process', 'process', 'domain_category']
    catalysis =['catalysis', 'generic_control_process', 'body_process', 'process', 'domain_category']
    facilitation =['facilitation', 'generic_control_process', 'body_process', 'process', 'domain_category']
    inhibition =['inhibition', 'generic_control_process', 'body_process', 'process', 'domain_category']
    stimulation =['stimulation', 'generic_control_process', 'body_process', 'process', 'domain_category']
    intrinsically_pathological_body_process =['intrinsically_pathological_body_process', 'body_process', 'process', 'domain_category']
    movement =['movement', 'body_process', 'process', 'domain_category']
    involuntary_movement =['involuntary_movement', 'movement', 'body_process', 'process', 'domain_category']
    voluntary_movement =['voluntary_movement', 'movement', 'body_process', 'process', 'domain_category']
    named_non_normal_process =['named_non_normal_process', 'body_process', 'process', 'domain_category']
    named_pathological_process =['named_pathological_process', 'named_non_normal_process', 'body_process', 'process', 'domain_category']
    vomiting =['vomiting', 'named_pathological_process', 'named_non_normal_process', 'body_process', 'process', 'domain_category']
    volitional_act =['volitional_act', 'body_process', 'process', 'domain_category']
    listening =['listening', 'volitional_act', 'body_process', 'process', 'domain_category']
    stripping =['stripping', 'volitional_act', 'body_process', 'process', 'domain_category']
    tasting =['tasting', 'volitional_act', 'body_process', 'process', 'domain_category']
    toileting =['toileting', 'violitional_act', 'body_process', 'process', 'domain_category']
    mental_process =['mental_process', 'process', 'domain_category']
    attitude =['attitude', 'mental_process', 'process', 'domain_category']
    unwillingness =['unwillingness', 'attitude', 'mental_process', 'process', 'domain_category']
    willingness =['willingness', 'attitude', 'mental_process', 'process', 'domain_category']
    consciousness =['consciousness', 'mental_process', 'process', 'domain_category']
    intrinsically_pathological_mental_process =['intrinsically_pathological_mental_process', 'consciousness', 'mental_process', 'process', 'domain_category']
    pathological_mental_process =['pathological_mental_process', 'mental_process', 'process', 'domain_category']
    perception =['perception', 'mental_process', 'process', 'domain_category']
    exteroception =['exteroception', 'perception', 'mental_process', 'process', 'domain_category']
    hearing =['hearing', 'exteroception', 'perception', 'mental_process', 'process', 'domain_category']
    smell =['smell', 'exteroception', 'perception', 'mental_process', 'process', 'domain_category']
    touch =['touch', 'exteroception', 'perception', 'mental_process', 'process', 'domain_category']
    vision =['vision', 'exteroception', 'perception', 'mental_process', 'process', 'domain_category']
    interoception =['interoception', 'perception', 'mental_process', 'process', 'domain_category']
    epigastric_fullness =['epigastric_fullness', 'interoception', 'perception', 'mental_process', 'process', 'domain_category']
    food_hunger =['food_hunger', 'interoception', 'perception', 'mental_process', 'process', 'domain_category']
    food_satiety =['food_satiety', 'interoception', 'perception', 'mental_process', 'process', 'domain_category']
    malaise =['malaise', 'interoception', 'perception', 'mental_process', 'process', 'domain_category']
    nausea =['nausea', 'interoception', 'perception', 'mental_process', 'process', 'domain_category']
    pain =['pain', 'interoception', 'perception', 'mental_process', 'process', 'domain_category']
    loin_pain =['loin_pain', 'pain', 'interoception', 'perception', 'mental_process', 'process', 'domain_category']
    tenderness =['tenderness', 'pain', 'interoception', 'perception', 'mental_process', 'process', 'domain_category']
    thirst =['thirst', 'interoception', 'perception', 'mental_process', 'process', 'domain_category']
    predisposition =['predisposition', 'mental_process', 'process', 'domain_category']
    non_body_process =['non_body_process', 'process', 'domain_category']
    phase =['phase', 'process', 'domain_category']
    reproductive_process =['reproductive_process', 'process', 'domain_category']


    galen= [thing, top_category, domain_category, generalised_structure, abstract_structure, logical_structure, \
    frame_of_reference, plane_of_reference, point_of_reference, reference_structure, ideas, plan, result, schema, signal, \
    speech, system, anatomical_system, digestive_system, genito_urinary_system, female_genito_urinary_system, \
    male_genito_urinary_system, functional_system, psychosocial_construct, psychological_construct, faith, memory, \
    social_organisation, physical_structure, linear_structure, displacement, solid_structure, body_structure,\
    abnormal_body_structure, body_part, named_internal_body_part, named_genital_tract_body_part, named_female_genital_tract_body_part,\
    uterus, vagina, named_male_genital_tract_body_part, testis, named_g_i_tract_body_part, anal_canal, colon, descending_colon, \
    intestine, intestine_or_stomach, rectum, rectum_or_colon, named_nervous_system_part, brain, cerebral_hemisphere,\
    named_sensory_part, fundus_oculi, inner_ear, named_urinary_tract_body_part, internal_urethral_orifice, ureter, urethra, \
    urinary_bladder, urinary_tract, named_surface_body_part, body_junctional_body_part, axilla, buttock, groin, hip, shoulder,\
    extremity_body_part, digit, finger, toe, main_extremity_body_part, extremity_joint_part, ankle, elbow, knee, wrist, \
    extremity_long_part, arm, forearm, leg, thigh, hand_or_foot, foot, hand, major_body_division, body_as_awhole, extremity,\
    head, head_and_neck, neck, trunk, named_head_surface_body_part, cheek, chin, ear, external_aspect_of_eye, face, forehead,\
    jaw, lip, mouth, nose, scalp, named_trunk_body_part, abdomen, back, breast, chest, flank, inguinal_region, \
    named_genital_surface_body_part, named_female_genital_surface_body_part, clitoris, female_external_genitalia, \
    mons_pubis, vulva, named_male_genital_surface_body_part, male_external_genitalia, penis, scrotum, nipple, pelvis, \
    perineum, pubes, thorax, umbilicus, surface_body_landmark, bregma, linea_alba, mid_clavicular_line, scapular_line,\
    waist, surface_opening, anus, external_auditory_meatus, introitus, mouth, naris, urethral_meatus, surface_configuration, \
    generic_body_structure, generic_body_surface_structure, built_structure, communication_structure, computer, paper, \
    telephone, device, animal_organ, animal_tissue, connecting_material, cutting_tool, knife, scissors, shaver, \
    fixation_device, nail, pin, plate, rod, screw, holding_tool, laboratory_machine, opening_tool, drill, knife, laser,\
    obturator, saw, scissors, solid_needle, trocar, probe, surgical_instrument, transport_device, catheter, tube,\
    generalised_substance, energy, heat, light, sound, substance, body_substance, intrinsically_pathological_body_substance,\
    named_body_substance, body_fluid, bile, feces, gastric_acid, milk, mucus, pus, saliva, tears, urine,\
    pathological_body_substance, chemical_substance, modifier_concept, aspect, feature, organism_feature, age,\
    anaesthesia, body_position, clinical_speciality, dependency, high_dependency, independant, low_dependency, \
    medium_dependency, grade_of_experience, gravidity, level_of_consciousness, agitated, coma, normal_level_of_consciousness,\
    reduced_level_of_consciousness, mobility, immobility, increased_mobility, normal_mobility, reduced_mobility, sex, \
    viability, process_feature, completeness, functional_feature, process_activity, process_activity_level, \
    decreased_activity_level, high_activity_level, increased_activity_level, low_activity_level, range, scope, sensitivity,\
    resistance, sensitivity, temporal_feature, chronicity, duration, finish_time, frequency, process_pattern, start_time, \
    time_of_occurrence, volitional_feature, urgency, quantity_feature, proportion, structural_feature, consistency, dimension,\
    absolute_measurement, area, length, level, mass, speed, volume, relative_measurement, quality, morphology, appearance, \
    colour, shape, sharpness, size, texture, position, anterior_posterior_position, lateral_position, medial_lateral_position,\
    proximal_distal_position, superior_inferior_position, upper_lower_position, visibility, substance_feature, \
    chemical_feature, concentration, physical_feature, pressure, temperature, physical_state, state, abstract_state, \
    level_state, absolute_level_state, high_level, low_level, moderate_level, undetected_level, change_in_level_state, \
    decreased, increased, unchanged, level_expectation_state, depressed_level, elevated_level, expected_level, unexpected_level,\
    depressed_level, elevated_level, trend_in_level_state, decreasing, increasing, stable, \
    positive_negative_state, equivocal, negative, positive, quality_state, average_quality, bad_quality, excellent_quality, \
    good_quality, poorquality, quantity, numeric_quantity, acceleration_value, area_value, concentration_value,\
    count_concentration_value, distance_value, flow_rate_value, frequency_value, mass_value, pressure_value, \
    speed_value, temperature_value, temporal_value, volume_value, ordinal_quantity, ratio, a_part, none, the_whole, \
    organism_state, age_state, adult, middle_aged_person, pensioner, young_adult, child, baby, infant, neonate, youth, teenager,\
    body_position_state, grade_of_experience_state, consultant, junior, senior, life_or_death_state, death, life, \
    pregnancy_state, gravid, non_gravid, process_state, activity_state, active, inactive, completeness_state, partial,\
    total, direction_state, forward, reverse, disease_state, severity_state, mild, moderate, severe, effectiveness_state,\
    effective, ineffective, sensitivity_state, resistant, sensitive, temporal_state, duration_state, long_time, short_time,\
    frequency_state, always, never, often, sometimes, usually, process_pattern_state, continuous, intermittent, persistent, \
    single, temporal_position_state, future, now, past, volitional_state, \
    urgency_state, non_urgent, elective, urgent, structural_state, consistency_state, firmness, rigidity,\
    softness, morphology_state, appearance_state, colour_state, shape_state, change_in_shape_state, contracted, \
    dilated, sharpness_state, blunt, sharp, size_state, absolute_size_state, large, small, change_in_size_state, larger, \
    smaller, texture_state, rough, smooth, position_state, absolute_position_state, change_in_position_state, substance_state,\
    status, abstract_status, countability_status, paired_or_unpaired_status, at_least_paired, unpaired, organism_status, \
    sex_status, female, male, process_status, structural_status, medical_status, normal_or_non_normal_status, non_normal,\
    abnormal, unusual, normal, pathological_or_physiological_status, pathological, physiological, visibility_status,\
    substance_status, general_level_of_specification, level_of_specification, at_least_partially_specified, \
    at_leastwell_specified, uniquely_specified, process_level_of_specification, at_least_partially_specified_process, \
    at_leastwell_specified_process, modality, existentiality, absence, presence, family_history, role, physiological_role, \
    social_role, slave_role, dominant_role, master_role, object_role, submissive_role, user_role, unit, composite_unit,\
    area_unit, concentration_unit, count_concentration_unit, dose_unit, flow_rate_unit, frequency_unit, pressure_unit, \
    speed_unit, volume_unit, organism, animal, process, behaviour, pattern_of_behaviour, avoidance, practice, reflex, \
    volitional_act, physiological_volitional_act, voluntary_movement, body_process, generic_control_process, catalysis,\
    facilitation, inhibition, stimulation, intrinsically_pathological_body_process, movement, involuntary_movement,\
    voluntary_movement, named_non_normal_process, named_pathological_process, vomiting, volitional_act, listening, \
    stripping, tasting, toileting, mental_process, attitude, unwillingness, willingness, consciousness, \
    intrinsically_pathological_mental_process, pathological_mental_process, perception, exteroception, hearing, smell,\
    touch, vision, interoception, epigastric_fullness, food_hunger, food_satiety, malaise, nausea, pain, loin_pain, \
    tenderness, thirst, predisposition, non_body_process, phase, reproductive_process]



    (For original implementation, see Sebastian Banks 'Concepts' at https://github.com/xflr6/concepts)


    


    
