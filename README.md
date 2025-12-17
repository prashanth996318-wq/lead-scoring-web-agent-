# lead-scoring-web-agent-
lead-scoring-web-agent/
│
├── README.md
├── requirements.txt
├── main.py
├── scoring.py
└── output/
#requirements.txt
pandas
python
def score_lead(row):
    score = 0

    if "Director" in row["Title"] or "Head" in row["Title"]:
        score += 30

    if row["Funding"] in ["Series A", "Series B"]:
        score += 20

    if row["Uses_Invitro"]:
        score += 15

    if row["Mentions_NAMs"]:
        score += 10

    if row["Location_Hub"]:
        score += 10

    if row["Recent_Publication"]:
        score += 40

    return min(score, 100)
import pandas as pd
from scoring import score_lead

data = [
    {
        "Name": "Dr. Emily Carter",
        "Title": "Director of Toxicology",
        "Company": "NovaHep Bio",
        "Funding": "Series B",
        "Uses_Invitro": True,
        "Mentions_NAMs": True,
        "Location_Hub": True,
        "Recent_Publication": True,
        "Email": "e.carter@novahep.com",
        "LinkedIn": "https://linkedin.com/in/emilycarter"
    },
    {
        "Name": "John Smith",
        "Title": "Junior Scientist",
        "Company": "EarlyLab",
        "Funding": "None",
        "Uses_Invitro": False,
        "Mentions_NAMs": False,
        "Location_Hub": False,
        "Recent_Publication": False,
        "Email": "john@earlylab.com",
        "LinkedIn": "https://linkedin.com/in/johnsmith"
    }
]


df = pd.DataFrame(data)
df["Probability"] = df.apply(score_lead, axis=1)
df = df.sort_values("Probability", ascending=False)

df.to_csv("output/leads.csv", index=False)
print("Output generated: output/leads.csv")
