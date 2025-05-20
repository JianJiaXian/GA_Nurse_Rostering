#  Genetic Algorithm for Nurse Rostering & Scheduling

This project applies a genetic algorithm to solve the **NP-hard nurse rostering problem**, with the goal of minimizing patient wait times and medical staff overtime, while respecting legal and preference constraints.

![Result Overview](./readme_img/results.png)

 For technical details, see the [Final Paper](https://github.com/chia-shein/Genetic_algorithm-nurse-rostering/blob/main/Final_Project_Paper.pdf)

---

##  Problem Description

Operating room scheduling differs from general ward scheduling due to:
- Uncertainty in patient operations
- Required prioritization of surgeries
- Staffing and rest regulations

Hospitals often rely on manual experience for shift planning, which may lead to:
- Long patient wait times
- Excessive staff overtime
- Suboptimal satisfaction and performance

This project uses **genetic algorithms** to provide objective, optimized schedules in reasonable time.

---

##  Dependencies

```bash
pip install deap seaborn
```

---

##  Genetic Representation

### Binary Encoding

Each shift is encoded as a 3-bit vector:

| Shift | Binary Code | Time           |
|-------|-------------|----------------|
| Early | [1, 0, 0]   | 07:00–15:00    |
| Afternoon | [0, 1, 0] | 15:00–23:00 |
| Night | [0, 0, 1]   | 23:00–07:00    |
| Off   | [0, 0, 0]   | —              |

<img src="./readme_img/Binary_Representation.png" width="400"/>

---

##  Problem Constraints

###  Hard Constraints
1. A nurse can only work one shift per day.
2. No consecutive shifts allowed (e.g., afternoon + night).
3. Each nurse must have at least **2 days off** per week.
4. **Minimum staff** required per shift:
   - Early: 2–4 nurses
   - Afternoon: 2–4 nurses
   - Night: 1–2 nurses

###  Soft Constraints
- Nurse schedule preferences

---

##  Data Format

### `data.json` contains:

- **Nurse Info**  
  ![](./readme_img/nurse_related.png)

- **Rostering Info**  
  ![](./readme_img/rostering_related.png)

- **Genetic Algorithm Config**  
  ![](./readme_img/GA_related.png)

---

##  Genetic Algorithm Overview

<img src="./readme_img/GA_flow.png" width="600"/>

### Main Parameters

| Days | Population | Generations | Crossover | Mutation |
|------|------------|-------------|-----------|----------|
| 31   | 300        | 500         | 0.9       | 0.1      |

### Operators

| Selection          | Crossover               | Mutation            |
|--------------------|-------------------------|---------------------|
| Random             | Two-point               | FlipBit             |
| Roulette Wheel     | Uniform (0.3–0.7)       | Gaussian Mutation   |
| Fitness Best/Worst | PMX, Order Crossover    | Inversion Mutation  |

---

##  Running the Code

```bash
python rostering.py
```

---

##  Experimental Results

### Result Summary

| ID | Selection | Crossover | Mutation | Min Fitness | Mean Fitness |
|----|-----------|-----------|----------|-------------|--------------|
| a  | Random    | Two-point | FlipBit  | 319.0       | 542.48       |
| f  | Fitness Best | Uniform(0.5) | FlipBit | **11.0** | **23.01**   |
| g  | Fitness Best | Uniform(0.7) | FlipBit | **8.0**  | **17.67**   |

> Best results were achieved using:  
> **Fitness Best Selection + Uniform Crossover (0.7) + FlipBit Mutation**

We export the final 31-day schedule to Excel for easy viewing by nurses.

<img src="./readme_img/Exp_flow.png" width="600"/>

---

##  Conclusion

- A binary genetic encoding was used to model the scheduling task.
- Domain-specific constraints and nurse preferences were embedded into the fitness function.
- The best-performing GA configuration significantly reduced scheduling conflicts and overtime.

---

##  Future Work

1. Extend to **multi-objective optimization**.
2. Improve handling of real-world dynamic events.
3. Tailor constraints for specific hospital policies.

---

##  Related Works

- T.C. Wong et al., *A Two-Stage Heuristic Approach for Nurse Scheduling*, 2014.
- Ziyi Chen et al., *Neural-Assisted Rostering*, 2022.
- H. Kawanaka et al., *Genetic Algorithm with Constraints for Nurse Scheduling*, 2001.
- More in the final report.

---

## 📂 Dataset

🔗 [International Nurse Rostering Benchmarks](http://www.schedulingbenchmarks.org/nrp/instances1_24.html)
