Query 1: Exact matches for Format B
SELECT FB.Name1, FB.Address, PM.Name1, PM.City, PM.State, PM.Zip5, PM.Zip4, PM.Address
FROM [Format B ] AS FB, [Postcard Party Master] AS PM
WHERE (((FB.Name1)=[PM].[Name1]) AND ((FB.Address)=[PM].[Address]) AND ((FB.Zip5)=[PM].[Zip5]) AND ((FB.City)=[PM].[City]) AND ((FB.State)=[PM].[State]));

Query 2: Exact matches for Format C
SELECT FC.Name1, FB.Address, PM.Name1, PM.City, PM.State, PM.Zip5, PM.Zip4, PM.Address
FROM [Format C ] AS FB, [Postcard Party Master] AS PM
WHERE (((FC.Name1)=[PM].[Name1]) AND ((FC.Address)=[PM].[Address]) AND ((FC.Zip5)=[PM].[Zip5]) AND ((FC.City)=[PM].[City]) AND ((FC.State)=[PM].[State]));


Query 3: Non-exact name matches, exact address/zipcode matches for Format B
SELECT FB.ID, FB.Name1, FB.Address,  FB.Zip5, PM1.Name1, PM1.Address, PM1.Zip5, PM1.ID
FROM [Format B ] AS FB, [Postcard Party Master] AS PM1
WHERE NOT EXISTS
(SELECT Name1 
FROM [Postcard Party Master] AS PM
WHERE FB.Name1 = PM.Name1) and
(PM1.Address=FB.Address AND
PM1.Zip5=FB.Zip5) 
UNION ALL SELECT FB.ID, FB.Name1, FB.Address,  FB.Zip5, PM1.Name1, PM1.Address, PM1.Zip5, PM1.ID
FROM [Format B ] AS FB, [Postcard Party Master] AS PM1
WHERE FB.Name1 = PM1.Name1 AND 
FB.Address = PM1.Address AND 
FB.Zip5 = PM1.Zip5 AND
FB.City = PM1.City AND 
FB.State = PM1.State
ORDER BY FB.ID;

Query 4: Non-exact name matches, exact address/zipcode matches for Format C
SELECT FC.ID, FC.Name1, FC.Address,  FC.Zip5, PM1.Name1, PM1.Address, PM1.Zip5, PM1.ID
FROM [Format C ] AS FC, [Postcard Party Master] AS PM1
WHERE NOT EXISTS
(SELECT Name1 
FROM [Postcard Party Master] AS PM
WHERE FC.Name1 = PM.Name1) and
(PM1.Address=FC.Address AND
PM1.Zip5=FB.Zip5) 
UNION ALL SELECT FC.ID, FC.Name1, FC.Address,  FC.Zip5, PM1.Name1, PM1.Address, PM1.Zip5, PM1.ID
FROM [Format C ] AS FC, [Postcard Party Master] AS PM1
WHERE FC.Name1 = PM1.Name1 AND 
FC.Address = PM1.Address AND 
FC.Zip5 = PM1.Zip5 AND
FC.City = PM1.City AND 
FC.State = PM1.State
ORDER BY FC.ID;
