# Hospital_DB

Backend for a small healthcare app built with Django, Django REST Framework and PostgreSQL. Users can register, log in with JWT, and manage patients, doctors and the mapping between them.

## Setup

1. Clone the repo and create a virtual environment

   python -m venv venv
   venv\Scripts\activate
   pip install -r requirements.txt

2. Create a PostgreSQL database named `healthcare_db`.

3. Copy `.env.example` to `.env` and fill in your values. Generate a secret key with:

   python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"

4. Run migrations and start the server

   python manage.py migrate
   python manage.py runserver

## Environment variables

SECRET_KEY, DEBUG, ALLOWED_HOSTS, DB_NAME, DB_USER, DB_PASSWORD, DB_HOST, DB_PORT

## API

All endpoints except register and login need the header `Authorization: Bearer <access_token>`.

| Method | Endpoint | Description |
|---|---|---|
| POST | /api/auth/register/ | Register with name, email, password |
| POST | /api/auth/login/ | Login with email and password, returns tokens |
| POST | /api/auth/token/refresh/ | Get a new access token |
| POST, GET | /api/patients/ | Add a patient, list your patients |
| GET, PUT, DELETE | /api/patients/<id>/ | Patient details, update, delete |
| POST, GET | /api/doctors/ | Add a doctor, list all doctors |
| GET, PUT, DELETE | /api/doctors/<id>/ | Doctor details, update, delete |
| POST, GET | /api/mappings/ | Assign a doctor to a patient, list your mappings |
| GET | /api/mappings/<patient_id>/ | Doctors assigned to a patient |
| DELETE | /api/mappings/<id>/ | Remove a mapping |

## Notes

- Patients are private to the user who created them. Another user gets a 404.
- Doctors are visible to every logged-in user, but only the creator can edit or delete them.
- The same doctor cannot be assigned to the same patient twice.
- In `/api/mappings/<id>/`, the id is a patient id for GET and a mapping id for DELETE.
