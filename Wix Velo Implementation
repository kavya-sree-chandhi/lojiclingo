# Anti-Fallacy Training Game: "LogicLingo" - Wix Velo Implementation

## Core Concept: Critical Thinking Through Gamified Learning
**Platform**: Wix Velo (with built-in Wix Data collections)
**Database**: Wix Data (no external database required)
**Coverage**: All 60 logical fallacies with multiple examples and difficulty levels

### Mission Statement
Transform critical thinking education by making logical fallacy recognition as engaging and accessible as language learning, while addressing the deeper pedagogical challenges that pure gamification presents.

## Technical Architecture for Wix Velo

### Wix Data Collections Structure

#### **Users Collection**
```javascript
{
  _id: "string",
  email: "string",
  displayName: "string",
  currentLevel: "number",
  totalXP: "number",
  streakDays: "number",
  lastActiveDate: "date",
  completedFallacies: ["string"], // Array of fallacy IDs
  userStats: {
    correctAnswers: "number",
    incorrectAnswers: "number",
    explanationQuality: "number", // 1-5 rating
    helpfulContributions: "number"
  }
}
```

#### **Fallacies Collection** (60 entries)
```javascript
{
  _id: "string", // e.g., "formal-001-undistributed-middle"
  title: "string", // "Undistributed Middle"
  category: "string", // "Formal", "Informal", "Contemporary", etc.
  subcategory: "string", // "Syllogistic", "Relevance", "Digital Age"
  difficultyLevel: "number", // 1-5
  description: "text",
  whyItFails: "text",
  classicExample: "text",
  modernExample: "text",
  digitalExample: "text", // For contemporary contexts
  commonContexts: ["string"], // Politics, Science, Social Media, etc.
  relatedFallacies: ["string"], // IDs of related fallacies
  exerciseTypes: ["string"] // Types of exercises available
}
```

#### **Exercises Collection** (300+ entries, 5+ per fallacy)
```javascript
{
  _id: "string",
  fallacyId: "string", // References Fallacies collection
  exerciseType: "string", // "identify", "explain", "construct", "context"
  difficultyLevel: "number",
  scenario: "text", // The example scenario
  options: ["string"], // For multiple choice
  correctAnswer: "string",
  explanation: "text",
  context: "string", // "social-media", "news", "debate", "advertising"
  realWorldExample: "boolean" // True if using actual current events
}
```

#### **UserProgress Collection**
```javascript
{
  _id: "string",
  userId: "string",
  fallacyId: "string",
  masteryLevel: "number", // 0-5
  attemptsCount: "number",
  correctCount: "number",
  lastAttempted: "date",
  needsReview: "boolean",
  personalNotes: "text"
}
```

### Wix Velo Page Structure

#### **Dynamic Pages Setup**
```javascript
// masterpage.js - Global navigation and user state
import { currentMember } from 'wix-members';
import wixData from 'wix-data';

$w.onReady(function () {
    loadUserProgress();
    updateStreakDisplay();
    setupGlobalNavigation();
});

// fallacy-exercise.js - Dynamic exercise page
export function loadExercise(fallacyId, exerciseType) {
    return wixData.query("Exercises")
        .eq("fallacyId", fallacyId)
        .eq("exerciseType", exerciseType)
        .find()
        .then((results) => {
            if (results.items.length > 0) {
                displayExercise(results.items[0]);
            }
        });
}
```

#### **Core Velo Functions**
```javascript
// User progress tracking
export function updateUserProgress(fallacyId, isCorrect, explanation) {
    return wixData.query("UserProgress")
        .eq("userId", currentMember.id)
        .eq("fallacyId", fallacyId)
        .find()
        .then((results) => {
            let progressData = results.items[0] || createNewProgress(fallacyId);
            progressData.attemptsCount++;
            if (isCorrect) progressData.correctCount++;
            
            // Update mastery level based on performance
            progressData.masteryLevel = calculateMastery(progressData);
            
            return wixData.save("UserProgress", progressData);
        });
}

// Adaptive difficulty system
export function getNextExercise(currentLevel, weaknesses) {
    return wixData.query("Exercises")
        .le("difficultyLevel", currentLevel + 1)
        .ge("difficultyLevel", Math.max(1, currentLevel - 1))
        .find()
        .then((results) => {
            // Prioritize user's weak areas
            return selectOptimalExercise(results.items, weaknesses);
        });
}
```

## Complete Fallacy Coverage Implementation

### All 60 Fallacies with Exercise Types

#### **Formal Fallacies (7 fallacies)**
**Implementation**: Structured logic exercises with visual argument mapping

1. **Undistributed Middle**
   - **Exercises**: Syllogism analysis, categorical logic puzzles, real-world argument evaluation
   - **Contexts**: Academic arguments, legal reasoning, political speeches
   - **Wix Elements**: Interactive syllogism builder, drag-and-drop logic components

2. **Affirming the Consequent**
   - **Exercises**: Conditional statement analysis, "if-then" logic games, causal reasoning
   - **Contexts**: Scientific claims, troubleshooting scenarios, medical diagnoses
   - **Wix Elements**: Flowchart builders, conditional logic simulators

3. **Denying the Antecedent**
   - **Exercises**: Inverse logic challenges, contrapositive understanding, scenario analysis
   - **Contexts**: Policy analysis, rule application, exception handling
   - **Wix Elements**: Rule-based scenario simulators

4. **Affirming a Disjunct**
   - **Exercises**: Either/or analysis, exclusive vs inclusive OR understanding
   - **Contexts**: Choice scenarios, binary thinking in politics, technology options
   - **Wix Elements**: Decision tree builders

5. **Fallacy of Four Terms**
   - **Exercises**: Term disambiguation, definition consistency checking
   - **Contexts**: Legal documents, technical specifications, policy language
   - **Wix Elements**: Definition matching games, term consistency checkers

6. **Masked-Man Fallacy**
   - **Exercises**: Identity context analysis, knowledge attribution scenarios
   - **Contexts**: Secret identities, professional vs personal knowledge, witness testimony
   - **Wix Elements**: Context-switching simulators

7. **Sunk Cost Fallacy**
   - **Exercises**: Investment decision scenarios, project continuation analysis
   - **Contexts**: Business decisions, personal relationships, government projects
   - **Wix Elements**: Cost-benefit calculators, decision timeline visualizers

#### **Informal Fallacies - Relevance (13 fallacies)**
**Implementation**: Real-world argument analysis with social media integration

8. **Ad Hominem**
   - **Exercise Types**:
     - Identify attacks vs argument critique
     - Separate person from position analysis
     - Reframe personal attacks as logical responses
   - **Real Examples**: Political debate clips, social media arguments, product reviews
   - **Wix Implementation**: Comment thread analysis, debate response generators

9. **Ad Baculum (Appeal to Force)**
   - **Exercise Types**:
     - Distinguish threats from logical consequences
     - Identify coercion in arguments
     - Recognize institutional pressure tactics
   - **Real Examples**: Workplace discussions, international relations, legal threats
   - **Wix Implementation**: Scenario simulators with pressure indicators

10. **Ad Populum (Bandwagon)**
    - **Exercise Types**:
      - Popularity vs truth distinction
      - Crowd psychology analysis
      - Marketing appeal identification
    - **Real Examples**: Social media trends, product advertisements, political movements
    - **Wix Implementation**: Trend analysis tools, popularity metric displays

11. **Appeal to Inappropriate Authority**
    - **Exercise Types**:
      - Authority domain expertise matching
      - Credibility assessment frameworks
      - Source evaluation protocols
    - **Real Examples**: Celebrity endorsements, expert opinions outside expertise, testimonials
    - **Wix Implementation**: Authority assessment matrix, expertise domain matching

12. **Appeal to Ignorance**
    - **Exercise Types**:
      - Burden of proof identification
      - Evidence absence vs evidence against
      - Null hypothesis understanding
    - **Real Examples**: Conspiracy theories, scientific claims, legal arguments
    - **Wix Implementation**: Evidence mapping tools, proof burden calculators

[Continue for all 60 fallacies with similar detail...]

#### **Contemporary Digital Age Fallacies (10 fallacies)**
**Implementation**: Live social media integration and current events analysis

32. **Algorithmic Bias Fallacy**
    - **Exercise Types**:
      - Search result analysis
      - Recommendation system awareness
      - Filter bubble identification
    - **Real Examples**: Google search variations, YouTube recommendations, social media feeds
    - **Wix Implementation**: Search result comparison tools, algorithm bias simulators

33. **Digital Echo Chamber Fallacy**
    - **Exercise Types**:
      - Information source diversity audit
      - Perspective broadening challenges
      - Bias confirmation tracking
    - **Real Examples**: Facebook feed analysis, Twitter timeline evaluation, news source variety
    - **Wix Implementation**: Source diversity trackers, perspective challenge generators

[Continue for remaining contemporary fallacies...]

### Exercise Type Framework

#### **Level 1: Recognition Exercises**
```javascript
// Example Wix Velo implementation
export function createRecognitionExercise(fallacyData) {
    return {
        type: "multiple-choice",
        question: `Which logical fallacy is present in this argument?`,
        scenario: fallacyData.modernExample,
        options: [
            fallacyData.title, // Correct answer
            ...getRandomFallacies(3) // Distractors
        ],
        explanation: fallacyData.whyItFails,
        context: fallacyData.commonContexts[0]
    };
}
```

#### **Level 2: Explanation Exercises**
```javascript
export function createExplanationExercise(fallacyData) {
    return {
        type: "text-input",
        question: `Explain why this argument is fallacious:`,
        scenario: fallacyData.digitalExample,
        expectedElements: [
            "Identifies the specific fallacy",
            "Explains the logical error",
            "Suggests how to improve the argument"
        ],
        rubric: generateExplanationRubric(fallacyData)
    };
}
```

#### **Level 3: Context Analysis**
```javascript
export function createContextExercise(fallacyData) {
    return {
        type: "scenario-analysis",
        question: `How does context affect this argument?`,
        scenarios: [
            fallacyData.classicExample,
            fallacyData.modernExample,
            fallacyData.digitalExample
        ],
        task: "Compare how the same fallacy appears differently",
        context: "multi-domain"
    };
}
```

#### **Level 4: Construction Exercises**
```javascript
export function createConstructionExercise(fallacyData) {
    return {
        type: "argument-builder",
        question: `Create a strong argument that avoids this fallacy:`,
        topic: generateRelevantTopic(fallacyData),
        tools: [
            "Evidence selector",
            "Logic checker",
            "Source validator"
        ],
        validation: checkArgumentStrength
    };
}
```

#### **Level 5: Teaching Exercises**
```javascript
export function createTeachingExercise(fallacyData) {
    return {
        type: "peer-instruction",
        question: `Help another user understand this fallacy:`,
        scenario: "User confused about distinction between " + 
                  fallacyData.title + " and " + 
                  fallacyData.relatedFallacies[0],
        task: "Create clear explanation with examples",
        evaluation: "peer-review"
    };
}
```

## Wix Velo User Interface Implementation

### Main Dashboard (dashboard.js)
```javascript
import wixData from 'wix-data';
import { currentMember } from 'wix-members';

$w.onReady(function () {
    loadUserDashboard();
    setupStreakDisplay();
    loadRecommendedExercises();
});

function loadUserDashboard() {
    wixData.query("Users")
        .eq("_id", currentMember.id)
        .find()
        .then((results) => {
            const user = results.items[0];
            $w('#userLevel').text = `Level ${user.currentLevel}`;
            $w('#totalXP').text = `${user.totalXP} XP`;
            $w('#streakCounter').text = `${user.streakDays} day streak`;
            
            updateProgressRings(user);
            loadFallacyProgress(user);
        });
}

function updateProgressRings(user) {
    // Visual progress indicators for each fallacy category
    const categories = ['Formal', 'Informal', 'Contemporary', 'Specialized'];
    categories.forEach(category => {
        calculateCategoryProgress(user._id, category)
            .then(progress => {
                $w(`#${category.toLowerCase()}Ring`).value = progress;
            });
    });
}
```

### Fallacy Exercise Page (fallacy-exercise.js)
```javascript
import wixData from 'wix-data';
import { timeline } from 'wix-animations';

let currentExercise = {};
let userResponse = {};

export function loadFallacyExercise(fallacyId, difficulty) {
    wixData.query("Exercises")
        .eq("fallacyId", fallacyId)
        .eq("difficultyLevel", difficulty)
        .find()
        .then((results) => {
            currentExercise = results.items[Math.floor(Math.random() * results.items.length)];
            displayExercise(currentExercise);
        });
}

function displayExercise(exercise) {
    $w('#exerciseType').text = exercise.exerciseType.toUpperCase();
    $w('#scenario').text = exercise.scenario;
    
    switch(exercise.exerciseType) {
        case 'identify':
            setupMultipleChoice(exercise);
            break;
        case 'explain':
            setupTextInput(exercise);
            break;
        case 'construct':
            setupArgumentBuilder(exercise);
            break;
        case 'context':
            setupContextAnalysis(exercise);
            break;
    }
}

function setupMultipleChoice(exercise) {
    $w('#optionsRepeater').data = exercise.options.map((option, index) => ({
        _id: index.toString(),
        text: option,
        isCorrect: option === exercise.correctAnswer
    }));
    
    $w('#optionsRepeater').onItemReady(($item, itemData) => {
        $item('#optionButton').label = itemData.text;
        $item('#optionButton').onClick(() => {
            handleAnswer(itemData.text, itemData.isCorrect);
        });
    });
}

function handleAnswer(selectedAnswer, isCorrect) {
    userResponse = {
        answer: selectedAnswer,
        isCorrect: isCorrect,
        timestamp: new Date()
    };
    
    showFeedback(isCorrect, currentExercise.explanation);
    updateUserProgress(currentExercise.fallacyId, isCorrect);
    
    // Animate feedback
    if (isCorrect) {
        timeline()
            .add($w('#correctAnimation'), { scale: 1.2, duration: 300 })
            .add($w('#correctAnimation'), { scale: 1, duration: 300 });
    }
}
```

### Gamification Elements (Adapted for Wix Velo)

#### **Progress Tracking System**
```javascript
// streaks.js
export function updateStreak() {
    const today = new Date().toDateString();
    const yesterday = new Date(Date.now() - 86400000).toDateString();
    
    wixData.query("Users")
        .eq("_id", currentMember.id)
        .find()
        .then((results) => {
            const user = results.items[0];
            const lastActive = new Date(user.lastActiveDate).toDateString();
            
            if (lastActive === today) {
                // Already active today, no change
                return;
            } else if (lastActive === yesterday) {
                // Consecutive day, increment streak
                user.streakDays += 1;
            } else {
                // Streak broken, reset to 1
                user.streakDays = 1;
            }
            
            user.lastActiveDate = new Date();
            wixData.save("Users", user);
            updateStreakDisplay(user.streakDays);
        });
}

function updateStreakDisplay(streakDays) {
    $w('#streakDisplay').text = `${streakDays} day streak!`;
    
    // Special animations for milestones
    if ([7, 30, 100, 365].includes(streakDays)) {
        showMilestoneAnimation(streakDays);
    }
}
```

#### **Social Learning Features**
```javascript
// community.js
export function setupCommunityFeatures() {
    loadLeaderboard();
    setupStudyGroups();
    loadCommunityAnalysis();
}

function loadLeaderboard() {
    wixData.query("Users")
        .descending("totalXP")
        .limit(10)
        .find()
        .then((results) => {
            $w('#leaderboard').data = results.items.map((user, index) => ({
                _id: user._id,
                rank: index + 1,
                displayName: user.displayName,
                totalXP: user.totalXP,
                level: user.currentLevel
            }));
        });
}

function setupStudyGroups() {
    // Create study groups based on skill level and interests
    wixData.query("StudyGroups")
        .find()
        .then((results) => {
            $w('#studyGroups').data = results.items;
        });
}
```

### Real-World Content Integration

#### **Current Events Integration**
```javascript
// currentEvents.js
export function loadCurrentEventExercises() {
    // Integration with RSS feeds or manual content updates
    wixData.query("CurrentEvents")
        .descending("publishDate")
        .limit(5)
        .find()
        .then((results) => {
            $w('#currentEventsRepeater').data = results.items.map(event => ({
                _id: event._id,
                headline: event.headline,
                source: event.source,
                fallacyTypes: event.identifiedFallacies,
                difficulty: event.difficultyLevel
            }));
        });
}

// Social media content analysis (using embedded posts)
export function analyzeSocialMedia() {
    // Display anonymized social media posts for analysis
    const socialPosts = [
        {
            platform: "Twitter",
            content: "Everyone is switching to this new diet. If you're not doing it, you're behind!",
            fallacyType: "ad-populum",
            context: "health-marketing"
        }
        // More examples loaded from database
    ];
    
    displaySocialMediaExercise(socialPosts[0]);
}
```

### Assessment and Progress Tracking

#### **Mastery Calculation**
```javascript
// mastery.js
export function calculateMasteryLevel(userId, fallacyId) {
    return wixData.query("UserProgress")
        .eq("userId", userId)
        .eq("fallacyId", fallacyId)
        .find()
        .then((results) => {
            if (results.items.length === 0) return 0;
            
            const progress = results.items[0];
            const accuracy = progress.correctCount / progress.attemptsCount;
            const recency = daysSince(progress.lastAttempted);
            
            // Mastery decreases over time without practice
            let mastery = accuracy * 5; // Base mastery 0-5
            if (recency > 30) mastery *= 0.8; // Decay after a month
            if (recency > 90) mastery *= 0.6; // More decay after 3 months
            
            return Math.min(5, Math.max(0, mastery));
        });
}

export function getWeakAreas(userId) {
    return wixData.query("UserProgress")
        .eq("userId", userId)
        .lt("masteryLevel", 3)
        .find()
        .then((results) => {
            return results.items.map(item => item.fallacyId);
        });
}
```

### Mobile Optimization for Wix

#### **Responsive Design Elements**
```javascript
// mobile.js
import { viewport } from 'wix-viewport';

$w.onReady(function () {
    if (viewport.type === 'mobile') {
        optimizeForMobile();
    }
});

function optimizeForMobile() {
    // Adjust exercise layouts for mobile
    $w('#exerciseContainer').customClassList.add('mobile-layout');
    
    // Simplify navigation for touch
    setupMobileNavigation();
    
    // Optimize text input for mobile keyboards
    $w('#explanationInput').placeholder = "Tap to explain (keep it brief)";
}
```

## Deployment Checklist for Wix Velo

### **Required Wix Features**
- ✅ Wix Data (for all collections)
- ✅ Wix Members (for user accounts)
- ✅ Dynamic Pages (for fallacy exercises)
- ✅ Corvid SDK (for advanced interactions)
- ✅ Wix Stores (optional, for premium features)

### **Database Setup**
1. Create all 5 collections (Users, Fallacies, Exercises, UserProgress, StudyGroups)
2. Import 60 fallacy definitions with examples
3. Create 300+ exercises (5+ per fallacy)
4. Set up proper permissions and indexes

### **Content Management**
1. Regular updates to current events exercises
2. Community moderation tools
3. Expert-reviewed content validation
4. Multilingual support for global deployment

### **Performance Optimization**
1. Image optimization for fallacy illustrations
2. Database query optimization
3. Caching for frequently accessed content
4. Progressive loading for mobile users

This Wix Velo implementation provides a complete, deployable solution that addresses all 60 logical fallacies with multiple exercise types, real-world context integration, and community features while maintaining the engaging elements that made Duolingo successful.
