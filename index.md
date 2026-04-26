---
# Do not edit the text between these lines!
layout: default
---

# Data Analysis for Continuous Improvement: Livestreaming Lectures
**Author:** Josue Garcia-Lopez (PID: 730880484)

## 1. Creative Ideation
Here are five ideas brainstormed to create value and improve the course:
1. The course should provide practice functions because it would help students reinforce concepts they find hard.
2. This course should post lectures online which would help students review content.
3. This course should readily update lecture slides for students who want to review content. 
4. This course should provide more opportunity for extra credit; this would encourage students to seek help knowing their efforts are being rewarded by the course. 
5. This course should have more exercises tailored for particular majors which provide the opportunity for students to gain insights on the workings of their field. 

## 2. Identifying Missing Data
* **Idea without sufficient data to analyze:** Idea 5, that this course should have exercises tailored for particular majors to give them the opportunity to gain insight into their career field. 
* **Suggestion for future data collection:** Something that could be done to collect data on this idea in the future is adding a question on the survey that asks whether someone would find exercises tailored to their major helpful.

## 3. Choosing the Analysis
* **Selected Idea:** This course should post lectures online which would help students review content.
* **Why this idea is most valuable:** I believe it would provide the most value to students if implemented. It would eliminate a couple of issues students face such as falling behind due to being unable to attend lectures. Additionally, it would give the chance for students who are struggling to keep up or don't fully grasp concepts to go back and rewatch segments of the lecture and learn at their own pace. Moreover, out of the brainstormed ideas, it has the most survey data that can be analyzed and would be the easiest to implement without having to alter the curriculum and structure of the course very much. 

---

## 4. Data Analysis

### Loading Data 
I loaded data from Izzi's survey CSV using `read_csv_rows` and `columnar`, then converted the relevant scale columns to integers for analysis.

```py
from data_utils import read_csv_rows, columnar, concat, head, select, count, convert_columns_to_int, filter_threshold

izzi = columnar(read_csv_rows("data/survey_izzi.csv"))
combined = convert_columns_to_int(izzi, ["add_livestream", "pace", "ls_effective"])
head(combined, 3)
```

## Select Relevant Columns
This portion of code narrows the dataset down to only the three columns relevant to my analysis. I use count to provide a summary of how many students gave a response to the livestream question and their score for simplification.

```py
selected = select(combined, ["add_livestream", "pace", "ls_effective"])
livestream_counts = count(selected["add_livestream"])
print("Livestream support distribution:", livestream_counts)
```

## Filtering with a Helper Function
I defined and used a helper function filter_threshold that filters the combined table to keep only rows where a column's value meets or exceeds a threshold. The list helps narrow it down to only students who strongly support livestreaming and having lectures accessible to view.

```py
strong_support = filter_threshold(combined, "add_livestream", 5)
print(f"Students who strongly want livestreaming (score >= 5): {len(strong_support['add_livestream'])}")
```

## Plot 1: Distribution of Livestream Support
Here I created a graph that shows the distribution of student responses to the add_livestream question to examine how strongly students feel about having lectures livestreamed.

```py
import seaborn as sns
import matplotlib.pyplot as plt

sns.countplot(x=selected["add_livestream"])
plt.title("Student Support for Livestreaming Lectures")
plt.xlabel("Support Level (1=Strongly Disagree, 7=Strongly Agree)")
plt.ylabel("Number of Students")
plt.show()
```

<img src="<custom-path>/static/imgs/Student_Support.png"   
width="500"/>


## Plot 2: Livestream and Course Pace
Here I made a graph that illustrates students' opinions on whether they consider the course fast-paced and how likely these individuals are to support livestreaming, as they may be most inclined to rewatch them to keep up with the material.


```py
pace_labels = []
for val in selected["pace"]:
    if val >= 5:
        pace_labels.append("Too Fast (5-7)")
    else:
        pace_labels.append("Okay/Slow (1-4)")

sns.boxplot(x=pace_labels, y=selected["add_livestream"])
plt.title("Livestream Support: Fast vs Okay/Slow Pace Students")
plt.xlabel("Perceived Course Pace")
plt.ylabel("Support for Livestreaming (1-7)")
plt.show()
```
<img src="<custom-path>/static/imgs/Student_Pace_Support.png"   width="500"/>

## Plot 3: Livestream vs. Lesson Video Effectiveness

In this graph, I explore whether students who find lesson videos effective would also support livestreaming, indicating they would value being able to review content on their own time.

```py
ls_labels = []
for val in selected["ls_effective"]:
    if val >= 5:
        ls_labels.append("Effective (5-7)")
    else:
        ls_labels.append("Not Effective (1-4)")

sns.boxplot(x=ls_labels, y=selected["add_livestream"])
plt.title("Livestream Support: Effectivity Levels of Lesson Videos")
plt.xlabel("Lesson Video Effectiveness")
plt.ylabel("Support for Livestreaming (1-7)")
plt.show()
```
<img src="<custom-path>/static/imgs/Lesson_Videos_Effectivity.png"   width="500"/>

## 5. Conclusion
My analysis dived into whether COMP110 should start livestreaming lectures and post them online. The data that has been gathered from the surveys supports this idea. In Plot 1, it can be seen that 393 out of 534 students gave a score of 5 or higher, and 201 gave the maximum score of 7. In Plot 2, it showed a strong trend that students who find the course pace too fast are more likely to support livestreaming since it would provide the opportunity to rewatch content. For Plot 3, it can be seen that students who find video lessons effective are more likely to be in favor of wanting livestreaming, indicating that students who value video content would find livestreaming as a beneficial component of the course.

A potential cost of implementing this idea is that it could possibly require additional technical resources or TA effort. Additionally, if students know lectures will be recorded, it may provide less incentive to attend class, which could lower attendance and participation rates. Furthermore, if this idea is implemented, results and effectiveness could be graded based on how sections in the past performed without recorded lectures to see whether this idea should become a permanent component of this class. Overall, the data supports the idea that COMP110 should implement livestreaming lectures.






<!-- This is a comment. Below, you'll see code for inserting an image. To make this image appear, update <custom-path>. To add an image, save it inside the imgs folder of this repository. -->
<img src="<custom-path>/static/imgs/Lesson_Videos_Effectivity.png"   width="500"/>

<img src="<custom-path>/static/imgs/Student_Pace_Support.png"   width="500"/>

<img src="<custom-path>/static/imgs/Student_Support.png"   
width="500"/>

## This is a small header

add text

```py
print("hello world")
```