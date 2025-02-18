#Перевірка чи немає пропущених комірок
SELECT count(*), count(OPER_NAME), count(OPER_CODE), count(D_REG), count(DEP), count(BRAND), count(MODEL), count(MAKE_YEAR), count(BODY), count(KIND), count(VIN)
FROM tz_opendata_z01012023_po01012024 tozp 

#Аналізування датасету. Первірка варіацій моделей, хоча кузов один і той самий, або різні виробничі площі
SELECT BRAND, MODEL, COUNT(VIN) as COUNT_BRAND
FROM tz_opendata_z01012023_po01012024 tozp 
WHERE MODEL LIKE 'LANOS' or MODEL LIKE 'RAV%' or MODEL LIKE 'ID.4%'
GROUP BY BRAND, MODEL
ORDER BY 2 desc;

#LANOS Різні виробники, модель одна й та сама, проводимо уніфікацію
UPDATE tz_opendata_z01012023_po01012024 
SET BRAND = 
    CASE 
        WHEN BRAND = 'DAEWOO' THEN 'ЗАЗ-DAEWOO'
        WHEN BRAND = 'ЗАЗ' THEN 'ЗАЗ-DAEWOO'
        WHEN BRAND = 'CHEVROLET' THEN 'ЗАЗ-DAEWOO'
        WHEN BRAND = 'FSO' THEN 'ЗАЗ-DAEWOO'
        ELSE BRAND
    END
WHERE MODEL LIKE 'LANOS';

#RAV4 Різні варіації одного кузова
UPDATE tz_opendata_z01012023_po01012024 
SET MODEL = 
    CASE 
        WHEN MODEL = 'RAV-4 HYBRID' THEN 'RAV4'
        WHEN MODEL = 'RAV4 EV' THEN 'RAV4'
        ELSE MODEL
    END
WHERE BRAND LIKE 'TOYOTA';

#ID.4 різні комплектації одного кузова
UPDATE tz_opendata_z01012023_po01012024 
SET MODEL = 
    CASE 
        WHEN MODEL = 'ID.4 PRO S' THEN 'ID.4'
        WHEN MODEL = 'ID.4 CROZZ PRIME' THEN 'ID.4'
        WHEN MODEL = 'ID.4 PRO' THEN 'ID.4'
        WHEN MODEL = 'ID.4X' THEN 'ID.4'
        WHEN MODEL = 'ID.4 CROZZ' THEN 'ID.4'
        WHEN MODEL = 'ID.4 CROZZ LITE PRO' THEN 'ID.4'
        WHEN MODEL = 'ID.4 CROZZ PRO' THEN 'ID.4'
        ELSE MODEL
    END
WHERE BRAND LIKE 'VOLKSWAGEN';


#BMW Уніфікація моделі 3 та 5, різні двигуни
UPDATE tz_opendata_z01012023_po01012024 
SET MODEL = 
    CASE 
        WHEN MODEL = '530D' THEN '5'
        WHEN MODEL = '530' THEN '5'
        WHEN MODEL = '530I' THEN '5'
        WHEN MODEL = '520I' THEN '5'
        WHEN MODEL = '520' THEN '5'
        WHEN MODEL = '520D' THEN '5'
        WHEN MODEL = '525D' THEN '5'
        WHEN MODEL = '525' THEN '5'
        WHEN MODEL = '525I' THEN '5'
        WHEN MODEL = '540' THEN '5'
        WHEN MODEL = '540I' THEN '5'
        WHEN MODEL = '528I' THEN '5'
        WHEN MODEL = '523I' THEN '5'
        WHEN MODEL = '528' THEN '5'
        WHEN MODEL = '528XI' THEN '5'
        WHEN MODEL = '535XI' THEN '5'
        WHEN MODEL = '535' THEN '5'
        WHEN MODEL = '535I' THEN '5'
        WHEN MODEL = '535D' THEN '5'
        WHEN MODEL = '520D' THEN '5'
        WHEN MODEL = '328I' THEN '3'
        WHEN MODEL = '328' THEN '3'
        WHEN MODEL = '320D' THEN '3'
        WHEN MODEL = '318' THEN '3'
        WHEN MODEL = '316' THEN '3'
        WHEN MODEL = '320' THEN '3'
        WHEN MODEL = '318I' THEN '3'
        WHEN MODEL = '320I' THEN '3'
        WHEN MODEL = '316I' THEN '3'
        WHEN MODEL = '318D' THEN '3'
        WHEN MODEL = '330I' THEN '3'
        WHEN MODEL = '335I' THEN '3'
        WHEN MODEL = '325I' THEN '3'
        WHEN MODEL = '330D' THEN '3'
        WHEN MODEL = '328XI' THEN '3'
        WHEN MODEL = '330XI' THEN '3'
        WHEN MODEL = '328D' THEN '3'
        WHEN MODEL = '325' THEN '3'
        ELSE MODEL
    END
WHERE BRAND LIKE 'BMW';

#Toyota. Уніфікація моделей
UPDATE tz_opendata_z01012023_po01012024 
SET MODEL = 
    CASE 
        WHEN MODEL = 'LAND CRUISER 200' THEN 'LAND CRUISER'
        WHEN MODEL = 'LAND CRUISER 150' THEN 'LAND CRUISER'
        WHEN MODEL = 'LAND CRUISER PRADO 150' THEN 'LAND CRUISER PRADO'
        WHEN MODEL = 'PRADO' THEN 'LAND CRUISER PRADO'
        WHEN MODEL = 'LAND CRUISER 120' THEN 'LAND CRUISER'
        WHEN MODEL = 'LAND CRUISER 100' THEN 'LAND CRUISER'
        WHEN MODEL = 'LAND CRUISER PRADO 120' THEN 'LAND CRUISER PRADO'
        WHEN MODEL = 'LAND CRUISER ' THEN 'LAND CRUISER'
        WHEN MODEL = 'C-HR HYBRID' THEN 'C-HR'
        WHEN MODEL = 'HIGHLANDER HYBRID' THEN 'HIGHLANDER'
        WHEN MODEL = 'YARIS HYBRID' THEN 'YARIS'
        ELSE MODEL
    END
WHERE BRAND LIKE 'TOYOTA';



#Перевірка
SELECT BRAND, MODEL, COUNT(VIN) as COUNT_BRAND
FROM tz_opendata_z01012023_po01012024 tozp 
WHERE MODEL LIKE 'LANOS' or MODEL LIKE 'RAV%' or MODEL LIKE 'ID.4%'
GROUP BY BRAND, MODEL
ORDER BY 2 desc;

#Аналізування операційних кодів
SELECT OPER_NAME, OPER_CODE, count(OPER_CODE) AS COUNT_CODE
FROM tz_opendata_z01012023_po01012024 tozp 
GROUP BY OPER_CODE, OPER_NAME
ORDER BY 3 desc;


#Створив таблицю tz_opendata_reg де в ручну для кожного департамента визначив регіон англійською мовою.


#Вибір кодів операцій які свідчать саме про купівлю авто (б/у чи нового)
SELECT OPER_NAME, OPER_CODE, D_REG, BRAND, MODEL, MAKE_YEAR, KIND, VIN, tz_opendata_reg.REG
FROM tz_opendata_z01012023_po01012024 tozp 
LEFT JOIN tz_opendata_reg ON tozp.DEP=tz_opendata_reg.DEP
WHERE OPER_CODE IN (315, 308, 100, 105, 70, 71, 69, 319, 99, 329, 72, 313, 310, 314, 331) AND KIND LIKE 'ЛЕГКОВИЙ'


#Вибір кодів операцій які свідчать саме про купівлю нового авто, виготовленого в 2022, 2023 році - new cars sales 2023
SELECT OPER_NAME, OPER_CODE, D_REG, BRAND, MODEL, MAKE_YEAR, KIND, VIN, tz_opendata_reg.REG
FROM tz_opendata_z01012023_po01012024 tozp 
RIGHT JOIN tz_opendata_reg ON tozp.DEP=tz_opendata_reg.DEP
WHERE OPER_CODE IN (99, 105, 69, 72) AND KIND LIKE 'ЛЕГКОВИЙ' AND MAKE_YEAR IN (2022, 2023)

#Вибрали бу авто (коди операцій), що були перерєстровані протягом 2023 року, у яких VIN код співпадає з VIN кодом машин, куплених вперше в 2023 році - second cars sales 2023 
SELECT BRAND, MODEL, MAKE_YEAR, VIN
FROM tz_opendata_z01012023_po01012024 tozp 
WHERE OPER_CODE IN (315, 308, 100, 70, 71, 319,329, 313, 310, 314, 331) AND VIN IN (
                                        SELECT VIN
                                        FROM tz_opendata_z01012023_po01012024 tozp 
                                        WHERE OPER_CODE IN (99, 105, 69, 72) AND KIND LIKE 'ЛЕГКОВИЙ' AND MAKE_YEAR IN (2022, 2023))



#Вибір кодів операцій які свідчать про купівлю авто (б/у) - used cars sales
SELECT OPER_NAME, COALESCE(OPER_CODE, 0), D_REG, BRAND, MODEL, MAKE_YEAR, KIND, VIN, tz_opendata_reg.REG
FROM tz_opendata_z01012023_po01012024 tozp 
RIGHT JOIN tz_opendata_reg ON tozp.DEP=tz_opendata_reg.DEP
WHERE (OPER_CODE IN (315, 308, 100, 70, 71, 319, 329, 313, 310, 314, 331) OR OPER_CODE IS NULL) AND (KIND LIKE 'ЛЕГКОВИЙ' OR KIND IS NULL)
