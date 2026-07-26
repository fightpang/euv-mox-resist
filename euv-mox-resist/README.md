# Predicting Bandgaps of Heavy Metal Oxides for EUV Photoresists

**Scientific Question:** Can a composition-based Random Forest model predict the electronic bandgap of heavy metal oxides across different metal families using the Materials Project dataset?

## 📊 Dataset Source & Access
The dataset consists of 7,786 unique, thermodynamically stable heavy metal oxides (containing Sn, Hf, Zr, Ti, Al, Zn, or Ni).
* **Source:** Data is dynamically retrieved from the **Materials Project** database using the `mp-api` python client.
* **Citation:** Jain, A., Ong, S. P., Hautier, G., Chen, W., Richards, W. D., Dacek, S., Cholia, S., Skinner, D., Ceder, G., Persson, K. A. (2013). Commentary: The Materials Project: A materials genome approach to accelerating materials innovation. *APL Materials*, 1(1), 011002.
* **Access Instructions:** You need a free API key from the Materials Project. Register at [https://nextgen.materialsproject.org/api](https://nextgen.materialsproject.org/api) to obtain your key.

## 🛠️ Step-by-step Setup Instructions

To ensure reproducibility, please follow these setup steps on a clean machine:

**1. Clone the repository**
```bash
git clone [https://github.com/fightpang/euv-mox-resist.git](https://github.com/fightpang/euv-mox-resist.git)
cd euv-mox-resist