ing the aggregation phase, revealing only the correct global average.
- **Trade-off**: High computational overhead but prevents "inference attacks" on specific hospital data distributions.

### 5.3 Differential Privacy (DP)
- **Mechanism**: Gradient Perturbation (Local Differential Privacy).
- **Privacy Guarantee**: Mathematically guarantees that the output of the model does not reveal whether any specific individual's data was included in the training set.
- **Implementation**:
    - **Clipping**: Gradient norms are clipped to a maximum threshold to limit the influence of outliers.
    - **Noise Injection**: Laplacian or Gaussian noise is added to the local updates before sending them to the server.
- **Trade-off**: Slightly reduces model accuracy (utility) in exchange for rigorous privacy protection ($(\epsilon, \delta)$-privacy).

---

## 6. Tools & Technologies

| Category | Tools Used | Purpose |
| :--- | :--- | :--- |
| **Programming Language** | **Python 3.10+** | Core logic for FL and Backend |
| **Web Framework** | **Flask** | REST API and Web Interface |
| **ML Libraries** | **Scikit-learn**, **Numpy**, **Pandas** | Model training and Data Manipulation |
| **Frontend** | **HTML5**, **CSS3**, **JavaScript** | Responsive User Interface |
| **Data Storage** | **CSV / In-Memory** | Simulating distributed databases |
| **Version Control** | **Git / GitHub** | Source code management |
| **Development** | **VS Code** | IDE |

---

## 5. Directory Structure

```plaintext
FL_goodUI/
├── app.py                          # Main Flask Application
├── blockchain_nft_system.py        # Custom Blockchain Simulation Class
├── federated_learning_engine.py    # Robust FL Implementation
├── requirements.txt                # Dependency List
├── templates/                      # HTML Templates
│   ├── dashboard.html
│   ├── patient_portal.html
│   ├── hospital_portal.html
│   └── ...
├── data/                           # (Generated) CSV Data Files
│   ├── patient_dataset.csv
│   ├── nft_metadata.csv
│   └── node_*.csv                  # Hospital-specific datasets
└── README.md                       # Project Documentation
```

---

## 5.1 Key Features

### Privacy-Preserving Architecture
- **No Data Centralization**: Patient data never leaves hospital servers
- **Federated Aggregation**: Only model parameters (weights/gradients) are shared
- **Consent Verification**: Real-time NFT-based consent checking before each training round
- **Audit Trail**: Immutable blockchain record of all consent changes

### Role-Based Access Control
- **Admin Portal**: Full system access, FL training control, comprehensive analytics
- **Hospital Portal**: View only institution-specific patients and statistics
- **Patient Portal**: View and manage personal medical records and consent settings
- **Ethereum Address Authentication**: MetaMask wallet-based login with Ganache

### Dynamic Consent Management
- **Real-Time Updates**: Consent changes immediately affect the next training round
- **Expiry Dates**: Time-limited consent with automatic revocation
- **Granular Control**: Per-patient consent settings
- **NFT Metadata**: Blockchain-backed consent tokens with immutable history

### Multi-Model Support
- **3 ML Algorithms**: Random Forest (95%), Neural Networks (92%), Gradient Boosting (94%)
- **3 Training Modes**: Standard FL, Secure Aggregation, Differential Privacy
- **Configurable Parameters**: Training rounds, model type, privacy mode
- **Real-Time Monitoring**: Live training progress, accuracy metrics, and loss curves
- **Feature Engineering**: 12 features including 4 derived interaction terms

---

## 5.2 API Endpoints

### Authentication
```
POST /api/auth/verify        # Verify Ethereum address and create session
POST /api/auth/check         # Validate current session against MetaMask
GET  /logout                 # Clear session and redirect to login
```

### Data Access (Role-Filtered)
```
GET  /api/patients           # Get patient data (filtered by role)
GET  /api/nodes              # Get hospital node statistics
GET  /api/blockchain         # Get blockchain transactions (last 50)
GET  /api/consent_analytics  # Get consent statistics and trends
```

### Federated Learning (Admin Only)
```
POST /api/start_training     # Start FL training with config
GET  /api/training_status    # Get current training progress
GET  /api/training_history   # Get completed training rounds
GET  /api/global_model       # Get global model state
```

### Consent Management
```
POST /api/update_consent     # Update patient consent status
```

---

## 5.3 Technical Implementation

### Backend Architecture
- **Framework**: Flask (Python 3.8+)
- **Session Management**: Server-side sessions with secret key
- **Data Storage**: CSV files (patient_dataset.csv, nft_metadata.csv)
- **Blockchain**: Python-based simulation with SHA-256 hashing
- **Authentication**: Ethereum address verification with role mapping
- **ML Libraries**: scikit-learn, pandas, numpy

### Frontend Stack
- **Templates**: Jinja2 (server-side rendering)
- **Styling**: Bootstrap 5 + Custom CSS
- **Charts**: Chart.js for real-time visualizations
- **Web3**: ethers.js for MetaMask integration
- **AJAX**: Axios for API calls

### Security Features
- **Address Verification**: Every page load validates MetaMask address
- **Session Clearing**: Automatic logout on account change
- **Role-Based Decorators**: `@require_auth()` on protected routes
- **Data Filtering**: Backend filters all queries by authenticated role
- **CORS Protection**: Configured for localhost only

### Data Flow
```
1. Patient → NFT Consent Update → Blockchain Transaction
2. Blockchain → NFT Metadata CSV → Hospital Filtered Datasets
3. FL Engine → Load Consented Data → Local Training
4. Hospital Nodes → Send Model Updates → Server Aggregation
5. Server → FedAvg Algorithm → Global Model Update
6. Dashboard → Real-time Metrics → User Interface
```

---

## 6. Setup and Installation

### Prerequisites
- Python installed (v3.8 or higher)
- Git
- Node.js and npm (for Ganache authentication)
- MetaMask browser extension (for authentication)

### Quick Start Commands

**Option 1: Basic Setup (No Authentication)**
```bash
pip install -r requirements.txt
python app.py
# Open http://localhost:5000
```

**Option 2: With Ganache Authentication (Recommended)**
```bash
# Terminal 1: Start Ganache
ganache --accounts 30 --port 7545 --deterministic

# Terminal 2: Setup and run Flask
pip install -r requirements.txt
python generate_address_mappings.py
python app.py
# Open http://localhost:5000/login and connect MetaMask
```

### Detailed Steps
1.  **Clone the Repository**
    ```bash
    git clone https://github.com/Varshith-BR/Federated-Learning-on-NFT-Owned-Patient-Data.git
    cd Federated-Learning-on-NFT-Owned-Patient-Data
    ```

2.  **Create Virtual Environment** (Optional but Recommended)
    ```bash
    python -m venv venv
    # Windows
    venv\Scripts\activate
    # Mac/Linux
    source venv/bin/activate
    ```

3.  **Install Dependencies**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Run the Application**
    ```bash
    python app.py
    ```

5.  **Access the Dashboard**
    - Open browser to `http://localhost:5000`

### 6.1 Ganache Authentication Setup (For Address-Based Access Control)

This project includes an **Ethereum address-based authentication system** using Ganache for role-based access control.

#### Prerequisites
- Node.js and npm installed
- MetaMask browser extension

#### Setup Steps

1.  **Install Ganache**
    ```bash
    npm install -g ganache
    ```

2.  **Start Ganache** (with deterministic addresses)
    ```bash
    ganache --accounts 30 --port 7545 --deterministic
    ```

3.  **Generate Address Mappings**
    ```bash
    python generate_address_mappings.py
    ```
    This creates `address_mapping.csv` with 30 pre-mapped accounts:
    - **Accounts 0-1**: Admins (full access)
    - **Accounts 2-9**: Hospitals (filtered data access)
    - **Accounts 10-29**: Patients (own records only)

4.  **Configure MetaMask**
    - Add network: `http://127.0.0.1:7545` (Chain ID: 1337)
    - Import test account private keys from Ganache

5.  **Access with Authentication**
    - Navigate to `http://localhost:5000/login`
    - Connect MetaMask wallet
    - System auto-detects role and redirects to appropriate portal

#### Role-Based Access
- **Admin** → `/training` (FL Training Dashboard)
- **Hospital** → `/hospital` (Hospital Portal with filtered data)
- **Patient** → `/patient` (Patient Portal with own records)

**📚 Detailed Documentation:** See `docs/` folder for comprehensive guides:
- `QUICK_START_AUTH.md` - Quick setup guide
- `GANACHE_AUTH_GUIDE.md` - Complete address mappings and private keys
- `URL_ACCESS_GUIDE.md` - All available endpoints
- `TEST_ACCOUNT_SWITCHING.md` - Testing instructions

---

## 7. Results & Observations

### 7.1 Model Performance (Real sklearn Training)

| Model | Training Accuracy | Validation Accuracy | Training Time |
|-------|------------------|---------------------|---------------|
| **Random Forest** | 100% | **95.60%** | ~2 seconds/round |
| **Neural Network (Deep MLP)** | ~98% | **~92%** | ~3 seconds/round |
| **Gradient Boosting** | ~99% | **~94%** | ~4 seconds/round |

### 7.2 Federated Learning Metrics
- **Participating Nodes**: 8 hospitals (all participating in each round)
- **Total Consented Data**: ~6,569 patient records per round
- **Global Model Convergence**: Stable after 2-3 rounds
- **Per-Hospital Accuracy Range**: 90-98% (varies by local data distribution)

### 7.3 Privacy & Consent
- **Privacy Preservation**: Zero leakage of raw patient rows between nodes.
- **Dynamic Consent**: Changes in the Patient Portal are immediately reflected in the next training round (verified via logs).
- **Consent Rate**: ~68.56% average across all hospitals
- **Blockchain Overhead**: Minimal impact on training start times (~1-2 seconds) due to efficient local simulation.

---

## 8. Future Scope

1.  **Integration with Ethereum/Hyperledger**: Replace the Python simulation with a real Web3 provider (Infura/Alchemy) and Solidity contracts.
2.  **Homomorphic Encryption**: Encrypt model weights during transit to prevent reverse-engineering of updates.
3.  **Mobile App**: Develop a React Native app for patients to manage consent on the go.
4.  **Differential Privacy**: Add noise to local updates to further guarantee individual privacy ($(\epsilon, \delta)$-differential privacy).

---

## 9. Conclusion

This project successfully demonstrates a working prototype of a privacy-first healthcare AI system. By empowering patients with NFT-based ownership, we bridge the trust gap between individuals and research institutions, paving the way for ethical and collaborative medical advancements.

---

**Developed for Major Project 2025**
*Contributors: Varshith B R*
