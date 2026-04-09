{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 49,
   "id": "79ed97c1-de82-418f-8170-5cbbffb15354",
   "metadata": {},
   "outputs": [],
   "source": [
    "def data_clean(df, target=None):\n",
    "    import pandas as pd\n",
    "    import numpy as np\n",
    "\n",
    "    df = df.copy()\n",
    "\n",
    "    for col in df.columns:\n",
    "\n",
    "        missing_ratio = df[col].isna().mean()\n",
    "        unique_ratio = df[col].nunique(dropna=True) / len(df)\n",
    "\n",
    "       \n",
    "        if missing_ratio > 0.6 or unique_ratio > 0.85:\n",
    "            df.drop(columns=[col], inplace=True)\n",
    "            continue\n",
    "\n",
    "        \n",
    "        if df[col].dtype == object:\n",
    "\n",
    "            if  unique_ratio > 0.7 or missing_ratio > 0.5:\n",
    "                df[col] = df[col].fillna(\"unknown\")\n",
    "            else:\n",
    "                if not df[col].mode().empty:\n",
    "                    df[col] = df[col].fillna(df[col].mode()[0])\n",
    "\n",
    "        \n",
    "        elif np.issubdtype(df[col].dtype, np.number):\n",
    "\n",
    "            if missing_ratio <= 0.4:\n",
    "                df[col] = df[col].fillna(df[col].median())\n",
    "            else:\n",
    "                df.drop(columns=[col], inplace=True)\n",
    "\n",
    "        \n",
    "        elif pd.api.types.is_datetime64_any_dtype(df[col]):\n",
    "\n",
    "            if missing_ratio <= 0.5:\n",
    "                df[col] = df[col].fillna(\"01-01-2025\")\n",
    "            else:\n",
    "                df.drop(columns=[col], inplace=True)\n",
    "\n",
    "    return df\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 54,
   "id": "db37c9ff",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>ID</th>\n",
       "      <th>Name</th>\n",
       "      <th>Dept</th>\n",
       "      <th>Salary</th>\n",
       "      <th>City</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>101</td>\n",
       "      <td>Rohit</td>\n",
       "      <td>IT</td>\n",
       "      <td>55000.0</td>\n",
       "      <td>Delhi</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>102</td>\n",
       "      <td>Mohit</td>\n",
       "      <td>Sales</td>\n",
       "      <td>NaN</td>\n",
       "      <td>Mumbai</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>103</td>\n",
       "      <td>Yash</td>\n",
       "      <td>IT</td>\n",
       "      <td>72000.0</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>104</td>\n",
       "      <td>Karan</td>\n",
       "      <td>HR</td>\n",
       "      <td>30000.0</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>105</td>\n",
       "      <td>NaN</td>\n",
       "      <td>Operations</td>\n",
       "      <td>52000.0</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>106</td>\n",
       "      <td>Manav</td>\n",
       "      <td>NaN</td>\n",
       "      <td>64000.0</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>107</td>\n",
       "      <td>Abhishek</td>\n",
       "      <td>IT</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>108</td>\n",
       "      <td>NaN</td>\n",
       "      <td>HR</td>\n",
       "      <td>45000.0</td>\n",
       "      <td>Pune</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>109</td>\n",
       "      <td>Keshav</td>\n",
       "      <td>NaN</td>\n",
       "      <td>58000.0</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>110</td>\n",
       "      <td>Arjun</td>\n",
       "      <td>IT</td>\n",
       "      <td>NaN</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "    ID      Name        Dept   Salary    City\n",
       "0  101     Rohit          IT  55000.0   Delhi\n",
       "1  102     Mohit       Sales      NaN  Mumbai\n",
       "2  103      Yash          IT  72000.0     NaN\n",
       "3  104     Karan          HR  30000.0     NaN\n",
       "4  105       NaN  Operations  52000.0     NaN\n",
       "5  106     Manav         NaN  64000.0     NaN\n",
       "6  107  Abhishek          IT      NaN     NaN\n",
       "7  108       NaN          HR  45000.0    Pune\n",
       "8  109    Keshav         NaN  58000.0     NaN\n",
       "9  110     Arjun          IT      NaN     NaN"
      ]
     },
     "execution_count": 54,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import numpy as np\n",
    "import pandas as pd\n",
    "di = {\n",
    "    \"ID\" : [101,102,103,104,105,106,107,108,109,110],\n",
    "    \"Name\" : [\"Rohit\" , \"Mohit\" , \"Yash\" , \"Karan\" , np.nan , \"Manav\" , \"Abhishek\" , np.nan , \"Keshav\", \"Arjun\"],\n",
    "    \"Dept\"  : [\"IT\" , \"Sales\" , \"IT\" , \"HR\" , \"Operations\" , np.nan , \"IT\" , \"HR\" , np.nan , \"IT\"],\n",
    "    \"Salary\" : [55000 ,np.nan , 72000 , 30000 , 52000 , 64000 , np.nan , 45000 , 58000 , np.nan],\n",
    "    \"City\" : [\"Delhi\" , \"Mumbai\" , np.nan , np.nan , np.nan , np.nan , np.nan , \"Pune\" , np.nan , np.nan]\n",
    "}\n",
    "df = pd.DataFrame(di)\n",
    "df"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 55,
   "id": "b869a89b",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "ID        0\n",
       "Name      2\n",
       "Dept      2\n",
       "Salary    3\n",
       "City      7\n",
       "dtype: int64"
      ]
     },
     "execution_count": 55,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "df.isnull().sum()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 56,
   "id": "6c7ae5e6",
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Name</th>\n",
       "      <th>Dept</th>\n",
       "      <th>Salary</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>Rohit</td>\n",
       "      <td>IT</td>\n",
       "      <td>55000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>Mohit</td>\n",
       "      <td>Sales</td>\n",
       "      <td>55000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>Yash</td>\n",
       "      <td>IT</td>\n",
       "      <td>72000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>Karan</td>\n",
       "      <td>HR</td>\n",
       "      <td>30000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>unknown</td>\n",
       "      <td>Operations</td>\n",
       "      <td>52000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>Manav</td>\n",
       "      <td>IT</td>\n",
       "      <td>64000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>Abhishek</td>\n",
       "      <td>IT</td>\n",
       "      <td>55000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>unknown</td>\n",
       "      <td>HR</td>\n",
       "      <td>45000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>Keshav</td>\n",
       "      <td>IT</td>\n",
       "      <td>58000.0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>Arjun</td>\n",
       "      <td>IT</td>\n",
       "      <td>55000.0</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "       Name        Dept   Salary\n",
       "0     Rohit          IT  55000.0\n",
       "1     Mohit       Sales  55000.0\n",
       "2      Yash          IT  72000.0\n",
       "3     Karan          HR  30000.0\n",
       "4   unknown  Operations  52000.0\n",
       "5     Manav          IT  64000.0\n",
       "6  Abhishek          IT  55000.0\n",
       "7   unknown          HR  45000.0\n",
       "8    Keshav          IT  58000.0\n",
       "9     Arjun          IT  55000.0"
      ]
     },
     "execution_count": 56,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "data_clean(df)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 58,
   "id": "499bb9a0",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0.2\n"
     ]
    }
   ],
   "source": [
    "missing_ratio = df[\"Dept\"].isna().mean()\n",
    "print(missing_ratio)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 59,
   "id": "7a5d65b6",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0.4\n"
     ]
    }
   ],
   "source": [
    "unique_ratio = df[\"Dept\"].nunique(dropna=True) / len(df)\n",
    "print(unique_ratio)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 53,
   "id": "9d46d840",
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "ID\n",
      "Name\n",
      "Dept\n",
      "Salary\n",
      "City\n"
     ]
    }
   ],
   "source": [
    "for col in df.columns:\n",
    "    print(col)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "id": "0da271cf",
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3 (ipykernel)",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.11.5"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 5
}
