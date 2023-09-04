# is-russian-verb-conjugation-irregular
Script to check whether or not a Russian verb conjugates as expected.

Uses data from [OpenRussian.org](www.openrussian.org) and their [database](https://app.togetherdb.com/db/fwoedz5fvtwvq03v/russian3/)

# Steps
1. Download verbs and word_forms csv tables from OpenRussian database
2. Remove rows from word_forms csv table not related to verb conjugation
3. For each verb, generate expected conjugation
4. Compare expected to actual conjugations
5. Future: clasify unexpected conjugations into groups (eg интерестовать - я интересую; рисовать - я рисую; ова changes to у in present tense)

# Output
A new csv file, with the following columns
- verb infinitive
- verb aspect
- classification (eg regular, -ова- verb, irregular, etc)
- ending type (-ть, -ить/-еть, or irregular - eg идти)
- three columns for each conjugation form (present/future forms: я, ты, он, мы, вы, они, past forms: он, она, оно, они, command forms: ты, вы)
   * expected
   * actual
   * expected == actual?

# Conjugating Verbs (using most basic rules)

Past Tense
- remove -ть ending, then add л, ла, ло, ли endings depending on subject

Present + Imperative Tense
- verbs fall into one of two categories

Group 1: Verbs that end in -ть (but not -ить/-еть)
1. Remove -ть
2. Add group 1 endings (see table below)

Group 2: Verbs that end in -ить/-еть
1. Remove -ить/-еть
2. Add group 2 endings (see table below)

|     | -ть | -ить/-еть |
|-----|-----|-----------|
| я   | ю   | у         |
| ты  | ешь | ишь       |
| он, она, оно  | ет  | ит        |
| мы  | ем  | им        |
| вы  | ете | ите       |
| они | ют  | ят        |
| ты command | й | и |
| вы command | йте | ите |

If a verb is reflexive, simply remove the -ся and conjugate like above. Then add the appropriate reflexive endings.

See a longer explanation [here](https://www.russianforeveryone.com/Rufe/Lessons/Course1/Grammar/GramUnit5/GramUnit5_2.htm)

TODO: add rules for participles and gerunds


# Expected Conjugation Algorithm in Plain English
* NOTE: You can see this algorithm applied step by step by running python3 conjugate.py with logging level set to DEBUG.

1. Set the verb stem equal to the full infinitive
2. Reflexive check; if stem ends in -ся, mark verb reflexive and remove the ending ся from the stem
3. Irregular check; if stem does not end in -ть, mark verb as irregular stem; unable to conjugate, so return early
4. Form past tense; add past tense endings to current stem (-л, -ла, -ло, -ли)
5. Accented ending check; if current stem ends in an accent, update present / future endings so first letter of ending is accented
6. Verb group check;
   * if current stem ends in -и or -е, mark as group two verb and remove и/е from stem
   * else, mark as group one verb
7. Form present/future + imperative tense; add group one/two endings to current stem
8. Reflexive additions; if verb is reflexive, add reflexive endings to all conjugated verb forms


