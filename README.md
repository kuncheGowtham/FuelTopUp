# FuelTopUp

On-demand fuel delivery platform — order fuel to your location and track the delivery in real time.

## Problem

Running out of fuel means finding a station, queuing, and losing time. For a stranded driver or a fleet operator running several vehicles, it is worse than an inconvenience. FuelTopUp inverts the model: instead of the vehicle going to the fuel, the fuel comes to the vehicle. The platform connects customers with nearby registered fuel providers and tracks the delivery from order to arrival.

## Tech stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| Frontend | HTML, CSS, JavaScript, Jinja2 templates |
| Database | SQLite |
| Location and mapping | Browser Geolocation API with map rendering |
| Authentication | Session-based login with hashed passwords |

## Features

**User management**
Registration and login for two account types — customers and fuel providers — with session handling and password hashing. Each role sees a different dashboard.

**Location and mapping**
The customer sets a delivery point, either by sharing browser location or entering an address. The system matches the request against providers operating in that area.

**Fuel ordering**
Customers choose fuel type and quantity, review the estimated cost, and place the order. Orders move through a defined set of states: placed, accepted, dispatched, delivered.

**Real-time tracking**
Once a provider accepts and dispatches, the customer can follow the delivery status and the vehicle's position until it arrives.

**Provider management**
Providers register, set their service area and availability, and accept or decline incoming orders from their dashboard.

**Notifications**
Status changes generate notifications at each stage — confirmation, dispatch, and delivery.

## Running locally

```bash
git clone https://github.com/gowtham2214/fueltopup.git
cd fueltopup

python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

pip install -r requirements.txt
python app.py
```

The application runs at `http://127.0.0.1:5000`.

### Environment variables

Create a `.env` file in the project root:

```
SECRET_KEY=your_secret_key_here
DATABASE_URL=sqlite:///fueltopup.db
MAPS_API_KEY=your_maps_api_key_here
```

`.env` is listed in `.gitignore` and is not committed to the repository.

## Project structure

```
fueltopup/
├── app.py                 # application entry point and route definitions
├── models.py              # database models
├── requirements.txt       # Python dependencies
├── static/
│   ├── css/
│   ├── js/
│   └── img/
├── templates/             # Jinja2 templates
│   ├── base.html
│   ├── login.html
│   ├── order.html
│   ├── tracking.html
│   └── provider_dashboard.html
└── README.md
```

## Design notes

**Why two account types rather than one.** Customers and providers need genuinely different views of the same order — a customer sees "where is my fuel," a provider sees "which jobs can I take." Separating the roles at the data layer kept the dashboards and permissions clean rather than stacking conditionals on a single user model.

**Order state machine.** Rather than a boolean "delivered" flag, each order carries an explicit status. This makes the tracking view a direct read of order state, and makes it straightforward to add stages later without restructuring.

**Session-based authentication.** For a server-rendered Flask application, sessions are simpler and sufficient compared to token-based auth, which matters more for separate frontend clients or mobile apps.

## Limitations and next steps

- **Payments.** Orders are confirmed without an online payment step. A gateway integration is the most significant missing piece for real use.
- **Tracking updates.** Position updates are polled rather than pushed. Moving to websockets would make tracking smoother and reduce unnecessary requests.
- **Provider verification.** Providers currently self-register with no verification or rating system. Any production version needs both, since trust is the core constraint in a fuel delivery marketplace.
- **Safety and compliance.** Transporting fuel is regulated. A real deployment would require licensing checks, approved containers, and handling protocols — treated here as out of scope for a prototype.

## Author

**Kunche Gowtham** — B.Tech Electronics & Communication Engineering, MLR Institute of Technology, Hyderabad

[GitHub](https://github.com/gowtham2214) · [LinkedIn](https://linkedin.com/in/kunche-gowtham-180950275) · gowthamkunche2214@gmail.com
