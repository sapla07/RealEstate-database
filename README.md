# RealEstate-database
database of real estate agency broker in which he store the data about the properties he had with them and owners of the property and it also store the data about the customer who purchase the land from broker 


CREATE DATABASE land ;
USE land ;

-- create table customer

CREATE TABLE customer
(
   cid INT PRIMARY KEY,
   cname VARCHAR(30),
   ccontact varchar(10)
);


-- create table broker

CREATE TABLE broker
(
    bid INT PRIMARY KEY,
    bname VARCHAR(30),
    bcontact varchar(10)
);


-- create table owner

CREATE TABLE owner
(
   oid INT PRIMARY KEY,
   oname varchar(30),
   ocontact varchar(10)
);

-- create table property 

CREATE TABLE property
(	pid INT PRIMARY KEY, address VARCHAR(100) NOT NULL, oid INT , 
	CONSTRAINT fk_pr FOREIGN KEY(oid) REFERENCES owner(oid),
	bid INT , CONSTRAINT fk_pr1 FOREIGN KEY(bid) REFERENCES broker(bid)
);

-- create table industrial

CREATE TABLE industrial
(
  pid INT,  CONSTRAINT fk_id FOREIGN KEY(pid)
             REFERENCES property(pid),
  area_inacre INT

);

-- create table residential

CREATE TABLE residential
(
 
  pid INT ,  CONSTRAINT fk_rs FOREIGN KEY(pid)
             REFERENCES property(pid),
  maint_cost INT,
  r_area_sqft INT
);

-- create table commercial

CREATE TABLE commercial
(
  
  pid INT , CONSTRAINT fk_cm FOREIGN KEY(pid)
             REFERENCES property(pid),
  c_area_sqft INT
);

-- create table to_sell

CREATE  TABLE to_sell
(
 
  pid INT , CONSTRAINT fk_ts FOREIGN KEY(pid)
             REFERENCES property(pid),
   buy_price INT 
);

-- create table to_rent

CREATE TABLE to_rent
(

  pid INT ,  CONSTRAINT fk_tr FOREIGN KEY(pid)
             REFERENCES property(pid),
   rent_rate INT
);




-- create table enquiry

CREATE TABLE enquiry
(
   cid INT ,  CONSTRAINT fk_eq FOREIGN KEY(cid)
             REFERENCES customer(cid),
   pid INT , CONSTRAINT fk_eq1 FOREIGN KEY(pid)
             REFERENCES property(pid),
   PRIMARY KEY(cid,pid)
);



-- ALTER command to add check,not null & default constraint 


ALTER TABLE  property ADD CHECK(pid>0);

ALTER TABLE property ADD CONSTRAINT dt_addr DEFAULT 'mumbai' FOR address;

ALTER TABLE to_sell ALTER COLUMN buy_price DECIMAL  (18,2) NOT NULL ;

ALTER TABLE to_rent ALTER COLUMN rent_rate DECIMAL  (18,2) NOT NULL ;

--DATA ENTRY

INSERT INTO broker VALUES(1,'NAGPAL','9876543210')
INSERT INTO broker VALUES(2,'PK AGENCY','8976543210')
INSERT INTO broker VALUES(3,'SURESH P','8979873210')
INSERT INTO broker VALUES(4,'NARESH R','8975432100')
INSERT INTO broker VALUES(5,'RAHEJA','9765252836')

INSERT INTO owner VALUES(1,'AMEY','9320399850')
INSERT INTO owner VALUES(2,'NIHAR','9322011195')
INSERT INTO owner VALUES(3,'SIDDHESH','9920260761')
INSERT INTO owner VALUES(4,'SUMEET','9323552916')
INSERT INTO owner VALUES(5,'CHIRAG','9321044572')

INSERT INTO property VALUES(1,'CHEMBUR',2,4)
INSERT INTO property VALUES(2,'ANDHERI',3,3)
INSERT INTO property VALUES(3,'ULHASNAGAR',4,1)
INSERT INTO property VALUES(4,'VASHI',1,2)
INSERT INTO property VALUES(5,'BANDRA',5,5)
INSERT INTO property VALUES(6,'GOREGAON',5,5)
INSERT INTO property VALUES(7,'COLABA',1,4)
INSERT INTO property VALUES(8,'THANE',5,2)
INSERT INTO property VALUES(9,'MULUND',3,1)
INSERT INTO property VALUES(10,'GHATKOPAR',2,3)

INSERT INTO industrial VALUES(8,10)
INSERT INTO industrial VALUES(2,15)

INSERT INTO residential VALUES(1,5000,1000)
INSERT INTO residential VALUES(3,3000,1500)
INSERT INTO residential VALUES(10,4000,1100)
INSERT INTO residential VALUES(7,7000,900)

INSERT INTO commercial VALUES(4,500)
INSERT INTO commercial VALUES(5,600)
INSERT INTO commercial VALUES(6,400)
INSERT INTO commercial VALUES(9,550)

INSERT INTO to_sell VALUES(2,200000)
INSERT INTO to_sell VALUES(5,1500000)
INSERT INTO to_sell VALUES(4,500000)
INSERT INTO to_sell VALUES(9,4500000)

INSERT INTO to_rent VALUES(1,20000)
INSERT INTO to_rent VALUES(3,5000)
INSERT INTO to_rent VALUES(6,20000)
INSERT INTO to_rent VALUES(7,25000)
INSERT INTO to_rent VALUES(8,10000)
INSERT INTO to_rent VALUES(10,30000)

INSERT INTO customer VALUES(1,'DALVI','9324632884')
INSERT INTO customer VALUES(2,'BHAVNA','7795251985')
INSERT INTO customer VALUES(3,'GAURAV','9920454547')
INSERT INTO customer VALUES(4,'GIRISH','9819299982')
INSERT INTO customer VALUES(5,'HARI','9029186169')

INSERT INTO enquiry VALUES(1,5)
INSERT INTO enquiry VALUES(1,7)
INSERT INTO enquiry VALUES(4,3)
INSERT INTO enquiry VALUES(2,10)
INSERT INTO enquiry VALUES(5,9)
INSERT INTO enquiry VALUES(3,9)
INSERT INTO enquiry VALUES(2,8)

--COMPLEX QUERY
--DISPLAY THE OWNER DETAILS OF RESIDENTIAL PROPERTY HAVING AREA > 1000 SQFT AND MAINTAINANCE COST < 5000

SELECT * FROM owner WHERE oid IN(SELECT oid FROM property WHERE pid IN(SELECT pid FROM residential WHERE r_area_sqft>1000 AND maint_cost<5000))

--DISPLAY DETAILS OF OWNERS WHO HAVE LISTED THEIR PROPERTY WITH 'NARESH R' BROKER

SELECT * FROM owner WHERE oid IN(SELECT oid FROM property WHERE bid =(SELECT bid FROM broker WHERE bname='NARESH R'))

--DISPLAY DETAILS OF OWNER AND HOW MANY PROPERTIES ARE LISTED BY THE PERSON WITH A BROKER.

SELECT O.oid,O.oname,O.ocontact,COUNT(P.PID)AS no_of_properties FROM owner O,property P WHERE O.oid=P.oid GROUP BY O.oid,o.oname,O.ocontact

--DISPLAY MAXIMUM RENT RATE OF PROPERTIES

SELECT pid,rent_rate FROM to_rent WHERE rent_rate=(SELECT MAX(rent_rate) FROM to_rent)

-- Display details of customer who have enquired about property at ghatkopar

SELECT * FROM customer WHERE cid IN( SELECT cid FROM enquiry WHERE pid = ( SELECT pid FROM property WHERE address= 'GHATKOPAR'));

--Recursive query

create table product (proid int primary key, Pname varchar(20),Prounder int foreign key references Product(proid))

insert into product values(1,'Mobile phone',NULL)
insert into product values (2,'smartphone',1)
insert into product values (3,'basic mobile',1)
insert into product values (4,'Samsung galaxy',2)
insert into product values (5,'htc one',2)
insert into product values (6,'nokia 3310',3)
insert into product values (7,'lava A7',3)

select * from product

--recursive execution 

with mphone AS (
	Select proid,Pname,Prounder from product where Prounder is NULL
	UNION ALL
	Select p.proid,p.Pname,p.Prounder from product as p INNER JOIN mphone as m ON p.Prounder= m.proid
	)

-- joins

-- display property which are for rent and their rent rate

 SELECT P.pid,P.address,R.rent_rate FROM property AS P , to_rent AS R WHERE P.pid=R.pid; 

 -- Display owner information and their property information
 
 SELECT P.pid,P.address, O.oid,O.oname,O.ocontact FROM property AS P, owner AS O WHERE O.oid=P.oid;

 --views

 -- create view having details of broker and property
 
 CREATE VIEW bropro(bid,bname,bcontact,pid,address) AS 
 SELECT B.bid,B.bname,B.bcontact,P.pid,P.address 
 FROM broker AS B , property AS P
 WHERE B.bid=P.bid;

 select * from bropro

  -- create view having details of the residential property 
 
 CREATE VIEW respro(pid,address,maint_cost,areainsqft) AS
 SELECT P.pid,P.address,R.maint_cost,R.r_area_sqft
 FROM property AS P, residential AS R
 WHERE P.pid=R.pid;

 select * from respro

 -- query on views
 
 -- Display broker details,property details and their rentrate
 
 SELECT B.bid,B.bname,B.bcontact,B.pid,B.address,R.rent_rate
 FROM bropro AS B , to_rent AS R
 WHERE B.pid=R.pid;

 --stored procedures
 
 -- Write a procedure to get the address of property when passed its pid
	create procedure listadd(@a int)
	AS
	BEGIN
	DECLARE @add varchar(20)
	SELECT @add=address from property where pid=@a
	print @add
	End
	Execute listadd 2

                                             

-- Write procedure to Get the owner's name by passing pid using output parameters

	create procedure owndet(@p int ,@own varchar(20) output)
	AS
	Begin
	Select @own=oname from owner where oid=(Select oid from property where pid=@p)
	End

	Begin
	Declare @own1 varchar(20)
	Execute owndet 3,@own1 output
	print @own1 
	End

                                             

-- Write nested procedure to find the owner and address both by passing pid
	
	create procedure ownadd(@id int)
	AS
	Begin
	declare @g varchar(20)
	print 'pid = '+cast(@id AS varchar(20))
	print ' address = '
	execute listadd @id
	execute owndet @id,@g output 
	print ' owner''s name = '+ @g
	End

	execute ownadd 3 


                          

--	create function to check if the property is for selling or renting
	create function prosell(@pro int) returns varchar(30)
	AS
	BEGIN
	declare @chk AS varchar(30)
	if exists(select * from to_sell where pid=@pro)
	begin 
		set @chk ='The property is for selling'
	end
	else
	begin
		set @chk = 'The property is for renting'
	end
	return @chk
	END

print dbo.prosell (2)

                         

--Create function for getting broker id who have listed a certain property

	create function chkbro(@prop int) returns int
	AS
	Begin
		Declare @bid int
		Select @bid=bid from property where pid = @prop
		return @bid
	end 

	Select bname from broker where bid = dbo.chkbro(4)

                                

