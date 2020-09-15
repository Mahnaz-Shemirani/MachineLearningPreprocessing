# MachineLearningPreprocessing

# PreprocessingMachineLearning

Machine learning - PreProcessing data in Python (extract penicillin resistance only)


import numpy as np

import pandas as pd

from sklearn import preprocessing 

#read excel file

anti_res = pd.read_excel('D:\RESOURCE PATH\FILE.xls', encoding='ansi')

#Stammnr is one of the column names

anti_res.rename(index=anti_res.Stammnr, inplace=True ) 

anti_res.drop('Stammnr', axis=1, inplace=True)

#delete columns that are not needed

anti_res.drop(['Slutsvarstext_kort','Analysbeskrivning','Kaenslighetsvaerde'], axis=1, inplace=True)

#select those which are indicated by 'R'

is_R=anti_res['Kaenslighetsgrupp']=='R' 

anti_R=anti_res[is_R]

#check the data

anti_R 

#define antibiotics of interest

antibiotics=['Penicillin V','Penicillin G','Piperacillin','Amoxicillin','Ampicillin', 'Mecillinam'] 

#extract antibiotics of interest, alex is a name

ant_alex=anti_R[anti_R.Beskrivning.isin(antibiotics)] 

#make dummy variable

anti_alex_data= pd.get_dummies(ant_alex) 

#check the data

anti_alex_data 

#remove column 'Kaenslighetsgrupp_R'

anti_alex_data.drop('Kaenslighetsgrupp_R', axis=1, inplace=True)

#check the data

anti_alex_data 

#remove duplicates

anti_alex_data.drop_duplicates(inplace=True) 

#check the data

anti_alex_data 

#change index name to isolate number='iso no'

anti_alex_data.index.name='iso no'

#rename columns

anti_data=anti_alex_data.rename(columns={'Registreringsdatum':'date','Beskrivning_Ampicillin':'Ampicillin',
                              'Beskrivning_Mecillinam':'Mecillinam','Beskrivning_Penicillin G':'Penicillin G', 
                              'Beskrivning_Penicillin V':'Penicillin V','Beskrivning_Piperacillin':'Piperacillin' } ) 

#group and sum antibiotics of each isolate number based on the date

anti_P=anti_data.groupby(['iso no','date']).agg('sum')

#save file

anti_P.to_excel(r'D:\PATH TO DESTINATION\anti_P.xlsx') 
