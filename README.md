# Student Dropout and Academic Success Prediction

A supervised machine-learning project that predicts student outcomes at admission and after each of the first two semesters. A separate binary model predicts Dropout or Graduate using both semesters and financial and economic context.

By **Yogesh Goyal** and **Tanish Kumar Singla**.

## Live demo

[Open the student dropout prediction demo](https://student-dropout-prediction-nine.vercel.app)

Choose a prediction stage, run the bundled example, or edit the fields and run a new prediction. The site displays the highest-scoring class and a score for each possible outcome. Use fictional profiles. Resolved outcomes can take longer to run because it uses TabICL.

## Dataset and results

The project uses the UCI *Predict Students’ Dropout and Academic Success* dataset. It contains **4,424 records** and **36 input features** covering student background, admission, semester performance, and financial or economic context.

| Prediction stage | Inputs | Outputs | Historical accuracy |
| --- | ---: | --- | ---: |
| Admission | 18 | Dropout, Enrolled, Graduate | 58.76% (520/885) |
| After semester 1 | 24 | Dropout, Enrolled, Graduate | 73.56% (651/885) |
| After semester 2 | 30 | Dropout, Enrolled, Graduate | 76.27% (675/885) |
| Resolved outcomes | 36 | Dropout or Graduate | **92.01% (668/726)** |

The binary model blends **75% cross-semester Explainable Boosting Machine** and **25% TabICL**. On the historical evaluation, it achieved **93.80% dropout precision**, **85.21% dropout recall**, and **89.30% dropout F1**. It identified 242 of 284 recorded dropouts.

The three-class stages use only the predictors assigned to that point in the academic timeline. The binary cohort excludes students whose recorded outcome was Enrolled. Evaluation records had prior project exposure, so the figures are exploratory. The 92.01% result applies to the binary task using both semesters.

[UCI dataset and attribution](https://doi.org/10.24432/C5MC89) · [Saved model registry](best_model_registry.json) · [Binary evaluation summary](results/systematic_benchmark/summary.json)

## Run predictions locally

From the project root, use Python 3.12 and keep the supplied model artifacts and pretrained checkpoint.

```bash
python3.12 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m src.predict_best --stage admission \
  --input examples/demo_student_admission.csv \
  --output admission_predictions.csv
```

Use `semester1`, `semester2`, or `binary_full` with its matching CSV in `examples/`. Keep the original column names. The output CSV contains the class scores and a `predicted_status` column.

For the Windows website, extract the separate **UCI Student Dropout Local Live Demo** package. Run `setup-local.cmd` once if the runtime is missing, then start the server and open [localhost:8769](http://127.0.0.1:8769/).

```bat
start-local.cmd --project "C:\path\to\student-dropout-Project"
```

## Repository structure

| Path | What it contains |
| --- | --- |
| `src/` | Preprocessing, training, inference, and experiment code |
| `data/` | UCI data, metadata, and the fixed split manifest |
| `examples/` | Example inputs for each prediction stage |
| `models/` | Saved models and pretrained checkpoint files |
| `results/` | Archived predictions, metrics, and experiment settings |
| `tests/` | Automated tests for the research code |

## Data and model attribution

The UCI dataset is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). See [third-party notices](THIRD_PARTY_NOTICES.md) for pretrained model terms and dependency attribution. The website is a research demo for understanding model behavior.
