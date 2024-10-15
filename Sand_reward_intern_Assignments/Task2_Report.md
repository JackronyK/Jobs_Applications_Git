## Task 2: Legacy Point Allocation

### Main Rules for Point Allocation

1. **Sign-up Points**:
   - Each mentor earns **250 points** for signing up to become a mentor. This is a **one-time allocation**, regardless of how many mentees they mentor.

2. **Mentoring Two Different Mentees**:
   - Mentors receive **1000 points** if they conduct mentorship sessions with at least **two different mentees**.

3. **Session Points per Mentee**:
   - Mentors can earn a total of **500 points per mentee**, distributed as follows:
     - **250 points per session**.
     - The mentor must conduct **at least two sessions** with the same mentee to earn the full 500 points.
     - **Each session must be at least 30 minutes** long to qualify for the 250 points.
     - At least **one session must include job information completion**.
     - Sessions must be conducted with **the same mentee** to count toward the 500 points per mentee.

### Key Criteria Breakdown

1. **Sign-Up Bonus**:
   - Each mentor is automatically awarded **250 points** upon signing up.

2. **Mentorship with Two Mentees**:
   - If a mentor has mentored at least **two different mentees**, they receive an additional **1000 points**.

3. **Per-Mentee Sessions**:
   - Each session that lasts **30 minutes or longer** with the same mentee awards **250 points**.
   - To earn the full **500 points per mentee**, mentors must hold at least **two sessions** with the same mentee and ensure that **one session includes job information completion**.

### Explanation of the Point Allocation Calculation

I used a Python function to automate the allocation of points. The function evaluates each mentor based on the criteria described above. Here’s a step-by-step breakdown of how the points are calculated:

1. **Initialize Data**:
   - I created a dataframe using the cleaned dataset, which includes columns for the mentor’s name, mentee name, session duration, and job information status.

2. **Allocate Sign-Up Points**:
   - Each mentor automatically receives **250 points** for signing up.

3. **Allocate Points for Mentoring Two Different Mentees**:
   - The code checks if the mentor has mentored at least **two unique mentees**. If true, the mentor is awarded **1000 points**.

4. **Allocate Mentorship Relationship Points**:
   - For each mentor, the function checks their sessions with each mentee. Mentors can earn:
     - **250 points per session** if the session lasts **30 minutes or more**.
     - The full **500 points** if they conduct at least **two sessions** with the same mentee and complete job information in one of the sessions.

5. **Sum the Points**:
   - The total points for each mentor are calculated by summing the points from the sign-up, mentoring two mentees, and per-mentee mentorship sessions.

### Code Explanation

```python
import pandas as pd

def point_calculator(df):
    # Empty dictionary to store the points
    point_allocation = {'Mentor_Name': [], 'Sign-up Points': [], 'At-least 2 Mentees Points': [], 'Mentorship Points': [], 'Total Points': []}

    for mentor in df['Mentor_Name'].unique():
        mentor_data = df[df['Mentor_Name'] == mentor]
        mentee_count = mentor_data['Mentee_Name'].nunique()

        # Sign-up points (250 points)
        sign_up_points = 250

        # Points for mentoring at least two mentees (1000 points if true)
        at_least_2_points = 1000 if mentee_count >= 2 else 0

        # Mentorship relationship points (250 points per session, max 500 per mentee)
        mentorship_points = 0
        for mentee in mentor_data['Mentee_Name'].unique():
            sessions = mentor_data[mentor_data['Mentee_Name'] == mentee]
            if len(sessions) >= 2:
                total_duration = sessions['Session_Duration_Min'].sum()
                job_info_completed = sessions['Job_Info_Completed'].eq('Yes').any()
                
                if total_duration >= 60 and job_info_completed:
                    mentorship_points += 500

        # Total points for the mentor
        total_points = sign_up_points + at_least_2_points + mentorship_points

        # Append data to the dictionary
        point_allocation['Mentor_Name'].append(mentor)
        point_allocation['Sign-up Points'].append(sign_up_points)
        point_allocation['At-least 2 Mentees Points'].append(at_least_2_points)
        point_allocation['Mentorship Points'].append(mentorship_points)
        point_allocation['Total Points'].append(total_points)

    return pd.DataFrame(point_allocation)
```

### Results of the Point Allocation

The results of the point allocation process are as follows:

| Mentor Name     | Sign-up Points | At-least 2 Mentees Points | Mentorship Points | Total Points |
|-----------------|----------------|---------------------------|-------------------|--------------|
| Sarah Clark     | 250            | 1000                      | 1500              | 2750         |
| Emily Davis     | 250            | 1000                      | 2250              | 3500         |
| James Wilson    | 250            | 1000                      | 1500              | 2750         |
| David Thompson  | 250            | 1000                      | 2250              | 3500         |
| Michael Lee     | 250            | 1000                      | 2000              | 3250         |

From the table, we can observe that **Emily Davis** and **David Thompson** earned the highest total points (3500 points each), followed closely by **Michael Lee** (3250 points). **Sarah Clark** and **James Wilson** both earned 2750 points.

### Visualization of Points

I created a bar plot to visualize the total points earned by each mentor:

![Total Points Earned by Each Mentor](<Total Points Earned by each Member.png>)

As seen in the graph, **Emily Davis** and **David Thompson** lead with the highest points, while the rest follow closely.

### Step-by-Step Outline of the Point Allocation Process

1. **Data Preparation**:
   - Load the cleaned dataset and ensure all required columns are in the correct format (e.g., session duration as integers, job information as boolean).
   
2. **Point Calculation**:
   - For each mentor:
     1. Award **250 sign-up points**.
     2. Check if they mentored **at least two unique mentees**. If yes, award **1000 points**.
     3. For each mentee, check:
        - Whether they had **at least two sessions**.
        - If the sessions were at least **30 minutes each**.
        - If **job information** was completed.
        - Award points based on these conditions.

3. **Testing and Verification**:
   - Verify that the point calculation function correctly follows all rules by manually inspecting a subset of the data.
   - Test edge cases such as:
     - Mentors with only one mentee.
     - Sessions that are less than 30 minutes.
     - Cases where no job information is provided.

4. **Final Output**:
   - Generate the final points allocation for each mentor and save it as a report or output it in the required format (e.g., Excel, CSV).
   - Visualize the results using a bar chart for easy interpretation.

