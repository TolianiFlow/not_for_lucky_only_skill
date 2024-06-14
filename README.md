СОЗДАНИЕ..

-- Create table Employees
CREATE TABLE Employees (
    id UUID PRIMARY KEY,
    last_name VARCHAR(50) NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    middle_name VARCHAR(50),
    birth_date DATE NOT NULL,
    phone VARCHAR(20) NOT NULL,
    address VARCHAR(100) NOT NULL
);

-- Create table Services
CREATE TABLE Services (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    price DECIMAL(10, 2) NOT NULL
);

-- Create table HistoryCost
CREATE TABLE HistoryCost (
    id SERIAL PRIMARY KEY,
    service_id INTEGER NOT NULL REFERENCES Services(id),
    old_price DECIMAL(10, 2) NOT NULL,
    new_price DECIMAL(10, 2) NOT NULL,
    change_date TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

-- Create table Clients
CREATE TABLE Clients (
    id UUID PRIMARY KEY,
    last_name VARCHAR(50) NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    middle_name VARCHAR(50),
    birth_date DATE NOT NULL,
    passport_series VARCHAR(10) NOT NULL,
    passport_number VARCHAR(10) NOT NULL,
    passport_issue_date DATE NOT NULL,
    passport_issued_by VARCHAR(50) NOT NULL,
    phone VARCHAR(20) NOT NULL,
    email VARCHAR(100) NOT NULL
);

-- Create table Rooms
CREATE TABLE Rooms (
    id SERIAL PRIMARY KEY,
    room_type VARCHAR(20) NOT NULL CHECK (room_type IN ('Одиночный', 'Двойной', 'Сьют', 'Люкс', 'Апартамент')),
    price DECIMAL(10, 2) NOT NULL
);

-- Create table GuestCards
CREATE TABLE GuestCards (
    id SERIAL PRIMARY KEY,
    client_id UUID NOT NULL REFERENCES Clients(id),
    room_id INTEGER NOT NULL REFERENCES Rooms(id),
    checkin_date DATE NOT NULL,
    checkout_date DATE NOT NULL,
    payment_type VARCHAR(20) NOT NULL
);

-- Create table AdditionalServices
CREATE TABLE AdditionalServices (
    id SERIAL PRIMARY KEY,
    guest_card_id INTEGER NOT NULL REFERENCES GuestCards(id),
    service_id INTEGER NOT NULL REFERENCES Services(id)
);

ЗАПОЛНЕНИЕ...

INSERT INTO Employees (id, last_name, first_name, middle_name, birth_date, phone, address)
VALUES
    ('123e4567-e89b-12d3-a456-426655440000', 'Иванов', 'Иван', 'Иванович', '1990-01-01', '1234567890', 'Москва, Ленина 10, 1, 1'),
    ('234e5678-f901-3456-b789-987654321000', 'Петров', 'Петр', 'Петрович', '1995-05-05', '9876543210', 'Санкт-Петербург, Невский 20, 2, 2'),
    ('345e6789-f012-4567-b890-123456789001', 'Смирнов', 'Смир', 'Смирнович', '1992-02-02', '1111111111', 'Москва, Ленина 11, 1, 1'),
    ('456e7890-f123-5678-c901-234567890001', 'Лебедев', 'Лебедь', 'Лебедевич', '1998-06-06', '2222222222', 'Санкт-Петербург, Невский 21, 2, 2');

INSERT INTO Services (id, name, price)
VALUES
    (1, 'Завтрак', 500.00),
    (2, 'Обед', 1000.00),
    (3, 'Ужин', 1500.00),
    (4, 'Массаж', 2000.00),
    (5, 'Фитнес', 3000.00);

INSERT INTO HistoryCost (id, service_id, old_price, new_price, change_date)
VALUES
    (1, 1, 400.00, 500.00, '2022-01-01'),
    (2, 2, 900.00, 1000.00, '2022-02-01'),
    (3, 3, 1200.00, 1500.00, '2022-03-01'),
    (4, 4, 1500.00, 2000.00, '2022-04-01'),
    (5, 5, 2500.00, 3000.00, '2022-05-01');

INSERT INTO Clients (id, last_name, first_name, middle_name, birth_date, passport_series, passport_number, passport_issue_date, passport_issued_by, phone, email)
VALUES
    ('345e6789-f012-4567-b890-123456789000', 'Сидоров', 'Сидор', 'Сидорович', '1992-03-03', '1234', '567890', '2010-01-01', 'Москва', '1234567890', 'sidorov@example.com'),
    ('456e7890-f123-5678-c901-234567890000', 'Кузнецов', 'Кузьма', 'Кузьмич', '1998-05-05', '5678', '901234', '2015-01-01', 'Санкт-Петербург', '9876543210', 'kuznetsov@example.com'),
    ('567e8901-f234-6789-d012-345678901000', 'Попов', 'Поп', 'Попович', '1995-07-07', '9012', '345678', '2012-01-01', 'Москва', '1111111111', 'popov@example.com'),
    ('678e9012-f345-7890-e123-456789012000', 'Сokolov', 'Соколь', 'Соколович', '1990-09-09', '1235', '678901', '2005-01-01', 'Санкт-Петербург', '2222222222', 'sokolov@example.com');

INSERT INTO Rooms (id, room_type, price)
VALUES
    (1, 'Одиночный', 2000.00),
    (2, 'Двойной', 3000.00),
    (3, 'Сьют', 5000.00),
    (4, 'Люкс', 8000.00),
    (5, 'Апартамент', 10000.00);

INSERT INTO GuestCards (id, client_id, room_id, checkin_date, checkout_date, payment_type)
VALUES
    (1, '345e6789-f012-4567-b890-123456789000', 1, '2022-01-01', '2022-01-05', 'Наличными'),
    (2, '345e6789-f012-4567-b890-123456789000', 2, '2022-01-10', '2022-01-15', 'Картой'),
    (3, '456e7890-f123-5678-c901-234567890000', 3, '2022-02-01', '2022-02-05', 'Наличными'),
    (4, '456e7890-f123-5678-c901-234567890000', 4, '2022-02-10', '2022-02-15', 'Картой'),
    (5, '567e8901-f234-6789-d012-345678901000', 1, '2022-03-01', '2022-03-05', 'Наличными'),
    (6, '567e8901-f234-6789-d012-345678901000', 2, '2022-03-10', '2022-03-15', 'Картой'),
    (7, '678e9012-f345-7890-e123-456789012000', 3, '2022-04-01', '2022-04-05', 'Наличными'),
    (8, '678e9012-f345-7890-e123-456789012000', 4, '2022-04-10', '2022-04-15', 'Картой'),
    (9, '345e6789-f012-4567-b890-123456789000', 5, '2022-05-01', '2022-05-05', 'Наличными'),
    (10, '456e7890-f123-5678-c901-234567890000', 5, '2022-05-10', '2022-05-15', 'Картой');

INSERT INTO AdditionalServices (id, guest_card_id, service_id)
VALUES
    (1, 1, 1),
    (2, 1, 2),
    (3, 2, 3),
    (4, 3, 1),
    (5, 3, 4),
    (6, 4, 2),
    (7, 5, 3),
    (8, 5, 5),
    (9, 6, 1),
    (10, 7, 4),
    (11, 8, 2),
    (12, 9, 3),
    (13, 10, 5);
