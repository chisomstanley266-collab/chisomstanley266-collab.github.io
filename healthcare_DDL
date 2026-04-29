-- Create database
CREATE DATABASE SunMedicalHospitalDB;
GO

USE SunMedicalHospitalDB;
GO

-- Patient table
CREATE TABLE Patient (
    Patient_ID INT IDENTITY(1,1) PRIMARY KEY,
    Name VARCHAR(100) NOT NULL CHECK (LEN(Name) > 1),
    Age TINYINT NOT NULL CHECK (Age BETWEEN 0 AND 120),
    Gender VARCHAR(10) NOT NULL CHECK (Gender IN ('Male', 'Female', 'Other')),
    Contact_Information VARCHAR(150) NOT NULL UNIQUE
);

-- Ward table
CREATE TABLE Ward (
    Ward_ID SMALLINT IDENTITY(1,1) PRIMARY KEY,
    Name VARCHAR(50) NOT NULL UNIQUE,
    Capacity TINYINT NOT NULL CHECK (Capacity > 0),
    Department VARCHAR(50) NULL
);

-- Staff table
CREATE TABLE Staff (
    Staff_ID INT IDENTITY(1,1) PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Position VARCHAR(50) NOT NULL,
    Department VARCHAR(50) NOT NULL,
    Specialty VARCHAR(50) NOT NULL
);

-- OP Type table
CREATE TABLE OP_Type (
    OPType_ID SMALLINT IDENTITY(1,1) PRIMARY KEY,
    Name VARCHAR(50) NOT NULL UNIQUE,
    Risk_Level VARCHAR(10) NOT NULL CHECK (Risk_Level IN ('Low', 'Medium', 'High')),
    Category VARCHAR(50) NULL
);

-- Admission table
CREATE TABLE Admission (
    Admission_ID INT IDENTITY(1,1) PRIMARY KEY,
    Patient_ID INT NOT NULL,
    Ward_ID SMALLINT NOT NULL,
    Date_Admitted DATE NOT NULL,
    Date_Discharged DATE NULL,
    Expected_Operation BIT NOT NULL DEFAULT 0,

    CONSTRAINT FK_Admission_Patient FOREIGN KEY (Patient_ID)
        REFERENCES Patient(Patient_ID),

    CONSTRAINT FK_Admission_Ward FOREIGN KEY (Ward_ID)
        REFERENCES Ward(Ward_ID),

    CONSTRAINT CHK_Discharge_Date CHECK (
        Date_Discharged IS NULL OR Date_Discharged >= Date_Admitted
    )
);

-- Operation table
CREATE TABLE Operation (
    Operation_ID INT IDENTITY(1,1) PRIMARY KEY,
    Patient_ID INT NOT NULL,
    Staff_ID INT NOT NULL,
    OPType_ID SMALLINT NOT NULL,
    Date DATETIME NOT NULL DEFAULT GETDATE(),
    Description TEXT NULL,

    CONSTRAINT FK_Operation_Patient FOREIGN KEY (Patient_ID)
        REFERENCES Patient(Patient_ID),

    CONSTRAINT FK_Operation_Staff FOREIGN KEY (Staff_ID)
        REFERENCES Staff(Staff_ID),

    CONSTRAINT FK_Operation_OPType FOREIGN KEY (OPType_ID)
        REFERENCES OP_Type(OPType_ID)
);

-- Observation table
CREATE TABLE Observation (
    Observation_ID INT IDENTITY(1,1) PRIMARY KEY,
    Patient_ID INT NOT NULL,
    Staff_ID INT NOT NULL,
    Date DATETIME NOT NULL DEFAULT GETDATE(),
    Note TEXT NULL,
    Operation_ID INT NULL,

    CONSTRAINT FK_Observation_Patient FOREIGN KEY (Patient_ID)
        REFERENCES Patient(Patient_ID),

    CONSTRAINT FK_Observation_Staff FOREIGN KEY (Staff_ID)
        REFERENCES Staff(Staff_ID),

    CONSTRAINT FK_Observation_Operation FOREIGN KEY (Operation_ID)
        REFERENCES Operation(Operation_ID)
);

-- Operation Staff bridge table
CREATE TABLE Operation_Staff (
    Operation_ID INT NOT NULL,
    Staff_ID INT NOT NULL,
    Role VARCHAR(20) NOT NULL CHECK (Role IN ('Surgeon', 'Anesthetist', 'Nurse', 'Assistant')),

    CONSTRAINT PK_Operation_Staff PRIMARY KEY (Operation_ID, Staff_ID, Role),

    CONSTRAINT FK_OperationStaff_Operation FOREIGN KEY (Operation_ID)
        REFERENCES Operation(Operation_ID),

    CONSTRAINT FK_OperationStaff_Staff FOREIGN KEY (Staff_ID)
        REFERENCES Staff(Staff_ID)
);

-- Dental Care table
CREATE TABLE Dental_Care (
    DentalCare_ID INT IDENTITY(1,1) PRIMARY KEY,
    Patient_ID INT NOT NULL,
    Staff_ID INT NOT NULL,
    Date DATETIME NOT NULL DEFAULT GETDATE(),
    Treatment_Type VARCHAR(100) NOT NULL,

    CONSTRAINT FK_DentalCare_Patient FOREIGN KEY (Patient_ID)
        REFERENCES Patient(Patient_ID),

    CONSTRAINT FK_DentalCare_Staff FOREIGN KEY (Staff_ID)
        REFERENCES Staff(Staff_ID)
);

-- Optical Care table
CREATE TABLE Optical_Care (
    OpticalCare_ID INT IDENTITY(1,1) PRIMARY KEY,
    Patient_ID INT NOT NULL,
    Staff_ID INT NOT NULL,
    Date DATETIME NOT NULL DEFAULT GETDATE(),
    Treatment_Type VARCHAR(100) NOT NULL,

    CONSTRAINT FK_OpticalCare_Patient FOREIGN KEY (Patient_ID)
        REFERENCES Patient(Patient_ID),

    CONSTRAINT FK_OpticalCare_Staff FOREIGN KEY (Staff_ID)
        REFERENCES Staff(Staff_ID)
);

-- Indexes for faster joins and filtering
CREATE INDEX IX_Admission_Patient_ID ON Admission(Patient_ID);
CREATE INDEX IX_Admission_Ward_ID ON Admission(Ward_ID);
CREATE INDEX IX_Observation_Patient_ID ON Observation(Patient_ID);
CREATE INDEX IX_Observation_Staff_ID ON Observation(Staff_ID);
CREATE INDEX IX_Operation_Patient_ID ON Operation(Patient_ID);
CREATE INDEX IX_DentalCare_Patient_ID ON Dental_Care(Patient_ID);
CREATE INDEX IX_OpticalCare_Patient_ID ON Optical_Care(Patient_ID);
