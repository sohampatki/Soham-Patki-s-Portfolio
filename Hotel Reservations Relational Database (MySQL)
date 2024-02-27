/*!40101 SET NAMES UTF8MB4 */;
/*!40101 SET SQL_MODE=''*/;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;
CREATE DATABASE /*!32312 IF NOT EXISTS*/`neu_soham_classicmodels` /*!40100 DEFAULT CHARACTER SET latin1 */;
USE `neu_soham_classicmodels`;

-- 1. To create database, we first create the tables without foreign keys.
-- TABLE 1: Creating Table 'Guest', primary key 'Guest_ID'.
CREATE TABLE `Guest` (
    `Guest_ID` INT NOT NULL,
    `Password` VARCHAR(255) NOT NULL,
    `First_Name` VARCHAR(255) NOT NULL,
    `Last_Name` VARCHAR(255) NOT NULL,
    `Email` VARCHAR(255) NOT NULL,
    PRIMARY KEY (`Guest_ID`)
) ENGINE=InnoDB;

-- TABLE 1: Inserting observations into table 'Guest'.
INSERT INTO Guest (Guest_ID, Password, First_Name, Last_Name, Email) VALUES
(101, 'pass1234', 'John', 'Doe', 'johndoe@example.com'),
(102, 'pass5678', 'Jane', 'Smith', 'janesmith@example.com'),
(103, 'pass9012', 'Alice', 'Johnson', 'alicej@example.com'),
(104, 'pass3456', 'Bob', 'Brown', 'bobbrown@example.com'),
(105, 'pass7890', 'Carol', 'Davis', 'carold@example.com'),
(106, 'pass2468', 'Eve', 'Martinez', 'evem@example.com'),
(107, 'pass1357', 'David', 'Taylor', 'davidt@example.com'),
(108, 'pass8642', 'Sara', 'Lee', 'saral@example.com'),
(109, 'pass9753', 'Brian', 'Clark', 'brianc@example.com'),
(110, 'pass6248', 'Nancy', 'Allen', 'nancya@example.com');

-- TABLE 2: Creating Table 'Building', primary key 'Building_ID'.
CREATE TABLE `Building` (
    `Building_ID` VARCHAR(10) NOT NULL,
    `Building_Name` VARCHAR(255) NOT NULL,
    PRIMARY KEY (`Building_ID`)
) ENGINE=InnoDB;

-- TABLE 2: Inserting observations into table 'Building'.
INSERT INTO Building (Building_ID, Building_Name) VALUES
('A', 'Main Tower'),
('B', 'Ocean Wing'),
('C', 'Garden Wing');

-- TABLE 3: Creating Table 'RoomType', primary key 'Room_Type_ID'.
CREATE TABLE `RoomType` (
    `Room_Type_ID` INT NOT NULL,
    `Type_Name` VARCHAR(255) NOT NULL,
    `Price` DECIMAL(10, 2) NOT NULL,
    `Capacity` INT NOT NULL,
    PRIMARY KEY (`Room_Type_ID`)
) ENGINE=InnoDB;

-- TABLE 3: Inserting observations into table 'RoomType'.
INSERT INTO RoomType (Room_Type_ID, Type_Name, Price, Capacity) VALUES
(1, 'Standard Room', 100.00, 2),
(2, 'Deluxe Room', 150.00, 4),
(3, 'Executive Suite', 250.00, 3),
(4, 'Family Suite', 200.00, 5),
(5, 'Penthouse Suite', 500.00, 6);

-- TABLE 4: Creating Table 'Payment', primary key 'Payment_ID'.
CREATE TABLE `Payment` (
    `Payment_ID` INT NOT NULL,
    `Card_Number` VARCHAR(19) NOT NULL,
    `Card_Type` VARCHAR(50) NOT NULL,
    `Billing_Address` TEXT NOT NULL,
    `Date` DATE NOT NULL,
    `Total_Cost` DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY (`Payment_ID`)
) ENGINE=InnoDB;

-- TABLE 4: Insert observations into the 'Payment' table. 
INSERT INTO Payment (Payment_ID, Card_Number, Card_Type, Billing_Address, Date, Total_Cost) VALUES
(10001, '1234567890123456', 'Visa', '123 Main St', '2023-03-15', 300.00),
(10002, '2345678901234567', 'MasterCard', '456 Elm St', '2023-03-15', 200.00),
(10003, '3456789012345678', 'Visa', '123 Main St', '2023-03-15', 150.00),
(10004, '4567890123456789', 'MasterCard', '789 Oak St', '2023-03-15', 350.00),
(10005, '5678901234567890', 'Visa', '123 Main St', '2023-03-16', 400.00),
(10006, '6789012345678901', 'MasterCard', '101 Pine St', '2023-03-16', 250.00),
(10007, '7890123456789012', 'Visa', '123 Main St', '2023-03-17', 225.00),
(10008, '8901234567890123', 'MasterCard', '202 Birch St', '2023-03-17', 275.00),
(10009, '9012345678901234', 'Visa', '303 Cedar St', '2023-03-18', 320.00),
(10010, '0123456789012345', 'MasterCard', '404 Spruce St', '2023-03-18', 180.00),
(10011, '1123456789012346', 'Visa', '505 Maple St', '2023-03-19', 290.00),
(10012, '2123456789012347', 'MasterCard', '606 Walnut St', '2023-03-19', 310.00),
(10013, '3123456789012348', 'Visa', '707 Chestnut St', '2023-03-20', 260.00),
(10014, '4123456789012349', 'MasterCard', '808 Aspen St', '2023-03-20', 280.00),
(10015, '5123456789012350', 'Visa', '909 Willow St', '2023-03-21', 300.00);

-- 2. Next, we create tables that have foreign keys.
-- TABLE 5: Creating Table 'Room', composite primary key 'Building_ID','Room_ID', foreign. key 'Room_Type_ID'.
CREATE TABLE `Room` (
    `Room_ID` INT NOT NULL,
    `Building_ID` VARCHAR(10) NOT NULL,
    `Room_Type_ID` INT NOT NULL,
    PRIMARY KEY (`Room_ID`, `Building_ID`),
    FOREIGN KEY (`Building_ID`) REFERENCES `Building`(`Building_ID`),
    FOREIGN KEY (`Room_Type_ID`) REFERENCES `RoomType`(`Room_Type_ID`)
) ENGINE=InnoDB;

-- TABLE 5: Inserting observations into table 'Room'.
INSERT INTO Room (Room_ID, Building_ID, Room_Type_ID) VALUES
(101, 'A', 1),
(102, 'A', 1),
(103, 'A', 2),
(104, 'A', 2),
(105, 'A', 3),  
(101, 'B', 3),
(102, 'B', 3),
(103, 'B', 4),
(104, 'B', 4),
(105, 'B', 5),  
(101, 'C', 2),
(102, 'C', 2),
(103, 'C', 1),
(104, 'C', 1),
(105, 'C', 5); 
 
-- TABLE 6: Creating Table 'Promotion', primary key 'Promotion_ID', foreign key 'Room_Type_ID'.
CREATE TABLE `Promotion` (
    `Promo_ID` VARCHAR(10) NOT NULL,
    `Percent_Discount` DECIMAL(5, 2) NOT NULL,
    `Description` TEXT,
    `Room_Type_ID` INT NOT NULL,
    PRIMARY KEY (`Promo_ID`),
    FOREIGN KEY (`Room_Type_ID`) REFERENCES `RoomType`(`Room_Type_ID`)
) ENGINE=InnoDB;

-- TABLE 6: Inserting observations into 'Promotion' table.
INSERT INTO Promotion (Promo_ID, Percent_Discount, Description, Room_Type_ID) VALUES
('SR320', 10.00, '10% off on Standard Rooms for stays of 3 days or more', 1),
('DR515', 15.00, '15% discount for Deluxe Rooms on bookings of at least 5 days', 2),
('ES220', 20.00, '20% off on Executive Suite bookings for a minimum of 2 days', 3);

-- TABLE 7: Creating Table 'Reservation', primary key 'Reservation_ID'.
-- Foreign keys 'Guest_ID', 'Building_ID', 'Room_ID', 'Payment_ID', 'Promo_ID'.
CREATE TABLE `Reservation` (
    `Reservation_ID` VARCHAR(10) NOT NULL ,
    `Guest_ID` INT NOT NULL,
    `Building_ID` VARCHAR(10) NOT NULL,
    `Room_ID` INT NOT NULL,
    `Payment_ID` INT NOT NULL,
    `Promo_ID` VARCHAR(10),
    `Start_Date` DATE NOT NULL,
    `End_Date` DATE NOT NULL,
    `No_of_guests` INT NOT NULL,
    PRIMARY KEY (`Reservation_ID`),
    FOREIGN KEY (`Guest_ID`) REFERENCES `Guest`(`Guest_ID`),
    FOREIGN KEY (`Payment_ID`) REFERENCES `Payment`(`Payment_ID`),
    FOREIGN KEY (`Promo_ID`) REFERENCES `Promotion`(`Promo_ID`),
    FOREIGN KEY (`Building_ID`, `Room_ID`) REFERENCES `Room`(`Building_ID`,`Room_ID`)
) ENGINE=InnoDB;

-- TABLE 7: Insert observations into 'Reservation' table.
INSERT INTO Reservation (Reservation_ID, Guest_ID, Building_ID, Room_ID, Payment_ID, Promo_ID, Start_Date, End_Date, No_of_guests) VALUES
('R1A2B3', 101, 'A', 101, 10001, 'SR320', '2023-03-01', '2023-03-04', 2),
('R1B2C3', 102, 'A', 102, 10002, 'SR320', '2023-03-05', '2023-03-10', 1),
('R1C2D3', 103, 'A', 103, 10001, null, '2023-03-11', '2023-03-14', 2),
('R1D2E3', 104, 'A', 104, 10003, null, '2023-03-15', '2023-03-18', 2),
('R1F2G3', 105, 'B', 101, 10003, null, '2023-03-23', '2023-03-26', 3),
('R1G2H3', 106, 'B', 102, 10004, null, '2023-03-27', '2023-03-30', 2),
('R1H2I3', 107, 'B', 103, 10005, null, '2023-04-01', '2023-04-04', 4),
('R1I2J3', 101, 'B', 104, 10002, null, '2023-04-05', '2023-04-08', 2),
('R1J2K3', 108, 'B', 105, 10004, null,'2023-04-09', '2023-04-12', 1),
('R1K2L3', 109, 'C', 101, 10005, 'ES220', '2023-04-13', '2023-04-16', 2),
('R1L2M3', 110, 'C', 102, 10006, null, '2023-04-17', '2023-04-20', 3),
('R1M2N3', 102, 'C', 103, 10007, 'SR320', '2023-04-21', '2023-04-24', 1),
('R1N2O3', 103, 'C', 104, 10008, 'SR320', '2023-04-25', '2023-04-28', 2),
('R1O2P3', 104, 'C', 105, 10009, 'DR515', '2023-04-29', '2023-05-02', 4),
('R1P2Q3', 105, 'C', 101, 10010, null, '2023-05-03', '2023-05-06', 2),
('R1Q2R3', 106, 'C', 102, 10011, 'DR515', '2023-05-07', '2023-05-10', 3),
('R1R2S3', 107, 'C', 103, 10012, null, '2023-05-11', '2023-05-14', 2),
('R1S2T3', 108, 'C', 104, 10013, null, '2023-05-15', '2023-05-18', 1),
('R1T2U3', 109, 'C', 105, 10014, null, '2023-05-19', '2023-05-22', 4);

-- TABLE 8: Creating Table 'Review', primary key 'Review_ID', foreign key 'Guest_ID'.
-- Check constraint applied to 'Stars' column to ensure values are between 1 and 5. 
CREATE TABLE `Review` (
    `Review_ID` INT NOT NULL,
    `Guest_ID` INT NOT NULL,
    `Review_Date` DATE NOT NULL,
    `Review` TEXT,
    `Stars` INT NOT NULL,
    PRIMARY KEY (`Review_ID`),
    FOREIGN KEY (`Guest_ID`) REFERENCES `Guest`(`Guest_ID`),
    CHECK (Stars BETWEEN 1 AND 5)
) ENGINE=InnoDB;

-- TABLE 8: Insert observations into 'Review' table.
INSERT INTO Review (Review_ID, Guest_ID, Review_Date, Review, Stars) VALUES
(0001, 101, '2023-03-05', 'Fantastic stay! The room was clean and well-furnished.', 5),
(0002, 102, '2023-03-10', 'Comfortable, but the room service was a bit slow.', 4),
(0003, 103, '2023-03-15', 'Had a great time. The staff were very friendly and helpful.', 5),
(0004, 104, '2023-03-20', 'Overall good, but the Wi-Fi connection was poor.', 3),
(0005, 105, '2023-03-25', 'Room was clean and well-maintained, but the breakfast buffet was very limited', 2);

-- TABLE 9: Creating Table 'Phone_Number', primary key 'Phone_ID', foreign key 'Guest_ID'.
CREATE TABLE Phone_Number (
    Phone_ID VARCHAR(10) NOT NULL,
    Phone_Number VARCHAR(15) NOT NULL,
    Guest_ID INT NOT NULL,
    PRIMARY KEY (Phone_ID),
    FOREIGN KEY (Guest_ID) REFERENCES Guest(Guest_ID)
) ENGINE=InnoDB;

-- TABLE 9: Insert observations into 'Phone_Number' table.
INSERT INTO phone_number (Phone_ID, Phone_Number, Guest_ID) VALUES
('P101', '555-1234', 101),
('P102', '555-5678', 102),
('P103', '555-9012', 103),
('P104', '555-3456', 104),
('P105', '555-7890', 105),
('P106', '555-2468', 106),
('P107', '555-1357', 107),
('P108', '555-8642', 108),
('P109', '555-9753', 109),
('P110', '555-6248', 110),
('P111', '555-1122', 101),
('P112', '555-2233', 102),
('P113', '555-3344', 103),
('P114', '555-4455', 104);

-- 3. Next, we create a trigger for the Reservation table.
-- Creating trigger 'CheckDateOverlap' to indicate error when new Reservation dates overlap with existing Reservation dates.
DELIMITER |

CREATE TRIGGER CheckDateOverlap
BEFORE INSERT ON Reservation
FOR EACH ROW
BEGIN
    DECLARE conflict_count INT DEFAULT 0;

    SELECT COUNT(*)
    INTO conflict_count
    FROM Reservation
    WHERE NEW.Room_ID = Room_ID 
      AND NEW.Building_ID = Building_ID
      AND (
        (NEW.Start_Date < End_Date AND NEW.End_Date > Start_Date) OR
        (NEW.End_Date > Start_Date AND NEW.Start_Date < End_Date)
      );

    IF conflict_count > 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Error: New reservation dates overlap with an existing reservation.';
    END IF;
END
|

DELIMITER ;

-- Checking whether the trigger functions as designed
INSERT INTO Reservation(Reservation_ID, Guest_ID, Building_ID, Room_ID, Payment_ID, Promo_ID, Start_Date, End_Date, No_of_guests) VALUES
('R1U2V3', 108, 'A', 101, 10015, 'SR320', '2023-02-27', '2023-03-02', 2)

/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;
/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;
/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;
/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;

-- 4. After database is created, we demonstrate examples of business questions the database can answer.
-- BUSINESS QUESTION 1: How many guests stayed in rooms of each room type in 2023?
SELECT rt.Type_Name, SUM(r.No_of_guests) AS Guests_Served
FROM RoomType rt
JOIN Room rm
ON rt.Room_Type_ID = rm.Room_Type_ID
JOIN Reservation r 
ON rm.Building_ID = r.Building_ID
AND rm.Room_ID = r.Room_ID
WHERE YEAR(r.Start_Date) = 2023
GROUP BY rt.Room_Type_ID
ORDER BY Guests_Served DESC;

-- BUSINESS QUESTION 2: What reviews and ratings did guests write after staying in rooms in Building 'A'?
SELECT g.Guest_ID, re.Review, re.Stars
FROM Building b
JOIN Reservation r 
ON b.Building_ID = r.Building_ID
JOIN Guest g
ON r.Guest_ID = g.Guest_ID
JOIN Review re
ON g.Guest_ID = re.Guest_ID
WHERE b.Building_ID = 'A'
ORDER BY re.Stars DESC;

-- BUSINESS QUESTION 3: How many times was each promotional offer used to make a reservation?
SELECT r.Promo_ID, p.Description, COUNT(r.Reservation_ID) AS No_Of_Bookings
FROM Reservation r
JOIN Promotion p
ON r.Promo_ID =  p.Promo_ID
WHERE r.Promo_ID IS NOT NULL
GROUP BY r.Promo_ID
ORDER BY No_Of_Bookings DESC;
