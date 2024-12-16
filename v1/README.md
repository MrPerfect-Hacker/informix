# IBM Informix Deployment in K3s Cluster

Dit project bevat de stappen om **IBM Informix** in een **K3s-cluster** te implementeren en configureren. Het doel van dit project is het automatisch deployen van een **IBM Informix 14.10** database in een **Kubernetes**-omgeving voor gebruik in trainingsomgevingen. Het project omvat ook de setup van **ODBC**-verbindingen om toegang te krijgen tot de database vanuit verschillende tools, zoals **DBeaver** en **Excel**.

## Vereisten

- **K3s Cluster** met één master en twee worker nodes (Ubuntu Server).
- **IBM Informix 14.10** Developer Edition.
- **kubectl** (Kubernetes CLI).
- **Docker** voor containerbeheer.
- **ODBC-compatibele tools** (zoals DBeaver, Excel) om verbinding te maken met de database.

## Architectuur

- **K3s Cluster**: Gebruikt om de Informix-database container te draaien met Kubernetes-pods, services en persistentie.
- **IBM Informix**: De Informix database draait in een Kubernetes pod met een persistent volume claim (PVC) voor gegevensopslag.
- **ODBC**: De ODBC-verbinding wordt geconfigureerd om toegang te krijgen tot de database vanuit verschillende applicaties.
  
## Stappen voor Implementatie

### 1. **K3s Cluster Setup**

- **K3s Installatie**: 
    - Zorg ervoor dat je **K3s** geïnstalleerd hebt op drie **Ubuntu Servers**, met één als **master** en de anderen als **worker** nodes.
    - Volg de K3s installatiestappen van de officiële K3s documentatie om K3s op te zetten.

- **Persistent Volumes**: 
    - Maak **PersistentVolume** en **PersistentVolumeClaim** aan voor de **Informix** database.
    - De opslag is geconfigureerd via de **default storage class**.

### 2. **IBM Informix Deployment**

**StatefulSet Deployment**: 
    - De Informix-database wordt gedeployed via een **StatefulSet** in Kubernetes om de persistentie van de gegevens en het netwerk consistent te houden.
    - De container draait met de **IBM Informix Developer Edition**.
    - De container gebruikt een init-container voor bestandspermissies.
      
**Service Configuration**:
    - De database is toegankelijk via een **NodePort** service om verbindingen van buiten het cluster mogelijk te maken.

    ### 3. **ODBC Configuratie**
 **ODBC Driver Installatie**:
    - Installeer de **IBM Informix ODBC driver** op je **Windows machine** via de **IBM Informix Client SDK**.

    **Configuratie voorbeeld voor `sqlhosts`**:
    ```plaintext
    informix        onsoctcp        *informix-7db68767b8-lvnsl         9088
    informix_dr     drsoctcp        *informix-7db68767b8-lvnsl         9089

  ### 5. **Testen en Beheer**

- **Verbindingstest**: Zorg ervoor dat je verbinding kunt maken met de database via DBeaver, SetNet32, of via ODBC-clients

  ---

## Samenvatting van Belangrijke Configuraties

- **Kubernetes** configuraties: Deployment YAML en Service YAML voor de Informix-container.
- **ODBC Configuratie**: DSN instellen met de juiste poort (30509) en serverinformatie.
- **SetNet32**: Verbindingsinstellingen voor Informix, inclusief hostnaam, service en poort.
- **SQLhosts**: Zorg ervoor dat de juiste poort (`9088`) is ingesteld voor de service `informix`.

---
