# Database-MySQL
Hotel database management system: Hotele-Gabi
Base composition:

1. The Hotele_Gabi database consists of 7 tables:
• Clients (containing information: name, surname, country, city, streets, house number, name of the document (it can be a passport, if the client does not have an ID card), series of documents (if he has a document, then series of evidence, if he has no passport number), document number (6-digit ID card number or passport number), PESEL number (11-digit PESEL number is optional because only Polish customers have a PESEL number and the database should be adapted to all customers) number, series, name of the identity document are obligatory ie passport or ID card There is also a gender column in the database, in the hotel, determining the gender helps to divide customers into rooms, who come as a group, a trip, etc .: women want to be alone in the room, men alone.
• Rooms, defines the room in which the client lives, i.e. the room numbers and the floor-number, the type of room people, e.g. value 2 means double
• Reservations, the table contains information about reservations made from when (check-in) and until (check-out), information whether the reservation has been made
• Hotels, there are several hotels in one country, hotels are located in many countries,
• Parking, determines how many parking spaces a given hotel has, each parking lot of a given hotel is assigned to the client who sleeps in it.
• Cities, hotels are in some city, but customers also live in some city, so I put the cities in a separate table and included foreign keys;
• Countries, there are several hotels in one country, but clients also have their own country of residence, I have put the names of the countries in a separate table.

2. Relationships between entities:
- customers book
- clients use the parking lot or not
- hotels have various car parks
- a few hotels are in one country
- clients come from different countries
- clients live in the city
- hotel is located in the city
- there may be several hotels or 1 in one city

3. Database functionality
 The database allows you to update records, i.e. change them,
add new records, delete records, e.g. when the customer resigns from the car park, or a change of name, surname is required, etc.
you can book rooms at different hotels in the multi-country chain
Provides information on people currently staying in hotels.
Check which client lives in which hotel, in which country and how many stars the hotels have

4. Sample database queries:
• Lists hotels by number of reservations and arranges them in descending order:
	
	SELECT
     	H.hotele_nazwa
    	,COUNT(*) AS [Liczba rezerwacji]
	FROM
    	hotele H
   	 JOIN pokoje P ON H.hotele_id = P.pokoje_hotele_id
    	JOIN rezerwacje RE ON P.pokoje_id = RE.rezerwacje_pokoje_id        
	GROUP BY
   	 H.hotele_nazwa
	ORDER BY
    	2 DESC
 
 • Selects those hotels with 3 to 5 stars inclusive from the table   
    	
	SELECT
     H.hotele_nazwa AS Nazwa
    	,H.hotele_adres AS Adres
    	,H.hotele_telefon AS Telefon
	,H.hotele_gwiazdki AS gwiazdki
	FROM
    HOTELE H
	WHERE
    	H.hotele_gwiazdki >= 3 AND H.hotele_gwiazdki <= 5
    
 • Shows which country has the most hotels by calculating the percentage of hotels in a given country to all hotels in the database and sorted in descending order.
 
    DECLARE @ile_hoteli numeric =
    (
        SELECT
            COUNT(*)
        FROM    
            hotele)   
	SELECT
     K.kraje_nazwa_kraju AS Kraj
    ,CAST(COUNT(*)*100/@ile_hoteli AS NUMERIC(4,2)) AS [%]
	FROM
    hotele H
    JOIN kraje K ON H.hotele_kraje_id = K.kraje_id    
	GROUP BY
    K.kraje_nazwa_kraju
	ORDER BY
    2 DESC 
    
    


    

