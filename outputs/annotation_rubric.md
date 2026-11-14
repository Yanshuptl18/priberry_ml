# Ground Truth Annotation Guide

This guide explains how human reviewers label skills from job descriptions. It defines what counts as a skill, how to separate required from preferred skills, and what to ignore.

## 1. Required Skills (Must-Have)

- **What it means**: Core skills that the candidate must have to do the job.
- **Common phrases in text**: "Required:", "Must have", "We need someone who knows", "You will use ... every day", "You must be comfortable with".
- **How to label**: Add the skill to `verified_required_skills` and save the exact sentence from the text as proof.

## 2. Preferred Skills (Nice-to-Have)

- **What it means**: Bonus or optional skills that help a candidate stand out, but are not strictly required.
- **Common phrases in text**: "Nice to have:", "Bonus if you have used", "Is a plus, not a must", "Good to have".
- **How to label**: Add the skill to `verified_preferred_skills` and save the exact sentence from the text as proof.

## 3. What to Ignore (Do Not Label)

- **Company Names and Industry Context**: Mentions of company names (like Orbit Logistics or Lumen Cloud Systems) or general business areas are not skill requirements.
- **General Work Culture Statements**: Sentences like "Good communication matters in this team" do not count as technical skill requirements.
- **Soft Skills Not on the Official List**: General traits like Leadership, Teamwork, or Time Management are excluded because they are not part of the 54 official canonical skills.
- **Negated Skills**: Tools the text says you will NOT use (for example, "no Java experience needed") must not be labeled as skills.
- **Broad Category Words**: Generic words like "API" or "Database" are not labeled as specific tools like "REST APIs" or "PostgreSQL" unless the specific tool is named.

## 4. Labeling Rules and Status

- **No Overlap**: A skill can be Required or Preferred, but never both in the same job description.
- **Review Status Values**:
  - `reviewed`: Label has been checked and verified against the text.
  - `pending`: Label still needs review.
  - `needs_adjudication`: Ambiguous case that needs a second reviewer to decide.
