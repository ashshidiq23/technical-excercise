# Dropout Enrollments Command

## **Overview**

This console command automates the process of marking enrollments as **DROPOUT** based on specific conditions.

---

## Setup Instructions

### Prerequisites

* Composer
* PHP 8.x
* Laravel 10+
* MySQL

### Clone the Repository

```bash
git clone https://github.com/ashshidiq23/technical-excercise
cd technical-excercise
```

### Install Dependencies

```bash
composer install
```

### Create a `.env` file

Copy the .env.example file to .env

```bash
cp .env.example .env
```

Set up your database connection in .env:

```dotenv
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=your_database
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

### Run Migration & Seeder

```bash
php artisan migrate --seed
```

---

## API Documentation

### Dropout Enrollments Command

This command drops out any enrollments that meet the following criteria:

* The enrollment deadline has passed.
* The student has no active exams (`IN_PROGRESS` status).
* The student has no pending submissions (`WAITING_REVIEW` status).

After that, the command will execute:
1. The enrollment status is updated to `DROPOUT`.
2. An activity log entry is created for tracking.

#### Usage
To execute the command, run:
```bash
php artisan enrollments:dropout
```

example output:
```text
Enrollments to be dropped out: 500000
Excluded from drop out: 39401
Final dropped out enrollments: 460599
default/App\Console\Commands\DropOutEnrollments: 30.00 MiB - 68719 ms
```

## Testing Instructions

#### Verify the dropout process manually:
* Before Running the Command, query enrollments that should be dropped based on the latest deadline:
```sql
SELECT * FROM enrollments
WHERE deadline_at <= (SELECT deadline_at FROM enrollments ORDER BY id DESC LIMIT 1)
AND status != 'DROPOUT';
```
* Run the command:
```bash
php artisan enrollments:dropout
```
* Check the enrollments table to ensure that the status of the relevant enrollments has been updated to `DROPOUT`:
```sql
SELECT * FROM enrollments WHERE status = 'DROPOUT';
```


