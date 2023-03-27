-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema mydb_exercises
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema mydb_exercises
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `mydb_exercises` ;
USE `mydb_exercises` ;

-- -----------------------------------------------------
-- Table `mydb_exercises`.`Medical`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb_exercises`.`Medical` (
  `id` INT NOT NULL,
  `Overtime_rate` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb_exercises`.`Specialist`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb_exercises`.`Specialist` (
  `id` INT NOT NULL,
  `Field_area` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb_exercises`.`Doctor`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb_exercises`.`Doctor` (
  `id` INT NOT NULL,
  `Name` VARCHAR(45) NOT NULL,
  `Date_of_birth` DATE NOT NULL,
  `Address` VARCHAR(45) NOT NULL,
  `Phone_number` INT ZEROFILL NOT NULL,
  `Salary` DOUBLE NOT NULL,
  `Medical_id` INT NOT NULL,
  `Specialist_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_Doctor_Medical_idx` (`Medical_id` ASC) VISIBLE,
  INDEX `fk_Doctor_Specialist1_idx` (`Specialist_id` ASC) VISIBLE,
  CONSTRAINT `fk_Doctor_Medical`
    FOREIGN KEY (`Medical_id`)
    REFERENCES `mydb_exercises`.`Medical` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Doctor_Specialist1`
    FOREIGN KEY (`Specialist_id`)
    REFERENCES `mydb_exercises`.`Specialist` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb_exercises`.`Patient`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb_exercises`.`Patient` (
  `id` INT NOT NULL,
  `Name` VARCHAR(45) NOT NULL,
  `Adress` VARCHAR(45) NOT NULL,
  `Phone_number` INT ZEROFILL NOT NULL,
  `Date_of_birth` DATE NOT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb_exercises`.`Appointment`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb_exercises`.`Appointment` (
  `id` INT NOT NULL,
  `Date` DATE NOT NULL,
  `Time` TIME NOT NULL,
  `Doctor_id` INT NOT NULL,
  `Patient_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_Appointment_Doctor1_idx` (`Doctor_id` ASC) VISIBLE,
  INDEX `fk_Appointment_Patient1_idx` (`Patient_id` ASC) VISIBLE,
  CONSTRAINT `fk_Appointment_Doctor1`
    FOREIGN KEY (`Doctor_id`)
    REFERENCES `mydb_exercises`.`Doctor` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Appointment_Patient1`
    FOREIGN KEY (`Patient_id`)
    REFERENCES `mydb_exercises`.`Patient` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb_exercises`.`Payment`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb_exercises`.`Payment` (
  `id` INT NOT NULL,
  `Details` VARCHAR(45) NOT NULL,
  `Method` ENUM('Card', 'Cash') NOT NULL,
  PRIMARY KEY (`id`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb_exercises`.`Bill`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb_exercises`.`Bill` (
  `id` INT NOT NULL,
  `Total` FLOAT NOT NULL,
  `Patient_id` INT NOT NULL,
  PRIMARY KEY (`id`),
  INDEX `fk_Bill_Patient1_idx` (`Patient_id` ASC) VISIBLE,
  CONSTRAINT `fk_Bill_Patient1`
    FOREIGN KEY (`Patient_id`)
    REFERENCES `mydb_exercises`.`Patient` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `mydb_exercises`.`Bill_has_Payment`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `mydb_exercises`.`Bill_has_Payment` (
  `id` INT NOT NULL,
  `Bill_id` INT NOT NULL,
  `Payment_id` INT NOT NULL,
  INDEX `fk_Bill_has_Payment_Payment1_idx` (`Payment_id` ASC) VISIBLE,
  INDEX `fk_Bill_has_Payment_Bill1_idx` (`Bill_id` ASC) VISIBLE,
  PRIMARY KEY (`id`),
  CONSTRAINT `fk_Bill_has_Payment_Bill1`
    FOREIGN KEY (`Bill_id`)
    REFERENCES `mydb_exercises`.`Bill` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Bill_has_Payment_Payment1`
    FOREIGN KEY (`Payment_id`)
    REFERENCES `mydb_exercises`.`Payment` (`id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

