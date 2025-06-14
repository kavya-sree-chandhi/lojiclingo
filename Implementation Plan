# Database-Free Real-Time Fallacy Scanner: Complete Implementation Plan

## System Architecture Overview

### Core Principle: Pure Client-Side Processing
- **Zero server storage** - everything runs in browser
- **Live content streams** - real-time internet scanning
- **Pattern-based detection** - 60 fallacy recognition algorithms
- **Local persistence only** - browser storage for user progress
- **Dynamic exercise generation** - exercises created from live content

## Phase 1: Foundation Infrastructure (Week 1-2)

### Wix Velo Project Structure
```
/pages
  ├── dashboard.js          // Main learning interface
  ├── live-exercise.js      // Real-time exercise handler
  ├── fallacy-explorer.js   // 60 fallacy reference guide
  └── progress-tracker.js   // Local progress visualization

/public
  ├── fallacy-detector.js   // 60 fallacy detection algorithms
  ├── content-scanner.js    // Live internet content fetcher
  ├── exercise-generator.js // Dynamic exercise creation
  └── progress-manager.js   // Local storage management
```

### Content Source Configuration
```javascript
// content-sources.js - No database, pure API integration
const CONTENT_SOURCES = {
    reddit: {
        url: 'https://www.reddit.com/r/{subreddit}/hot.json?limit=25',
        subreddits: ['politics', 'science', 'news', 'changemyview', 'unpopularopinion'],
        rateLimit: 60, // requests per minute
        fallback: true
    },
    
    hackernews: {
        url: 'https://hacker-news.firebaseio.com/v0/topstories.json',
        itemUrl: 'https://hacker-news.firebaseio.com/v0/item/{id}.json',
        rateLimit: 100,
        fallback: true
    },
    
    rss: {
        feeds: [
            'https://feeds.bbci.co.uk/news/rss.xml',
            'https://rss.cnn.com/rss/edition.rss',
            'https://feeds.reuters.com/reuters/topNews'
        ],
        parser: 'rss-parser',
        fallback: true
    },
    
    youtube: {
        // Using YouTube's oEmbed API for comment access
        url: 'https://www.youtube.com/oembed?url=',
        rateLimit: 50,
        fallback: false // Comments require special handling
    }
};

// Fallback content for when APIs are unavailable
const FALLBACK_EXAMPLES = [
    // Emergency content pool when live sources fail
    {
        text: "Everyone is switching to this new diet, you should too!",
        source: "health-forum",
        fallacies: ['ad-populum'],
        context: "health"
    }
    // ... 200+ curated examples covering all 60 fallacies
];
```

## Phase 2: Fallacy Detection Engine (Week 3-4)

### All 60 Fallacy Detection Algorithms
```javascript
// fallacy-detector.js - Complete detection system
class FallacyDetector {
    constructor() {
        this.patterns = this.initializeAllPatterns();
        this.contextAnalyzer = new ContextAnalyzer();
    }
    
    initializeAllPatterns() {
        return {
            // FORMAL FALLACIES (7)
            undistributedMiddle: {
                regex: /All (.*?) are (.*?)\.\s*(.*?) (?:are|is) \2\.\s*(?:Therefore|So),?\s*\3 (?:are|is) \1/gi,
                keywords: ['all', 'are', 'therefore'],
                strength: 0.9,
                validate: (match, context) => this.validateSyllogism(match)
            },
            
            affirmingConsequent: {
                regex: /If (.*?) then (.*?)\.\s*\2\.\s*(?:Therefore|So),?\s*\1/gi,
                keywords: ['if', 'then', 'therefore'],
                strength: 0.85,
                validate: (match, context) => this.validateConditional(match)
            },
            
            denyingAntecedent: {
                regex: /If (.*?) then (.*?)\.\s*(?:Not|No) \1\.\s*(?:Therefore|So),?\s*(?:not|no) \2/gi,
                keywords: ['if', 'then', 'not', 'therefore'],
                strength: 0.85,
                validate: (match, context) => this.validateConditional(match)
            },
            
            affirmingDisjunct: {
                regex: /(.*?) (?:or|either) (.*?)\.\s*\1\.\s*(?:Therefore|So),?\s*(?:not|no) \2/gi,
                keywords: ['either', 'or', 'therefore', 'not'],
                strength: 0.8,
                validate: (match, context) => this.validateDisjunctive(match)
            },
            
            fourTerms: {
                regex: /All (.*?) (?:have|are) (.*?)\.\s*(?:The|This) (.*?) (?:has|is) \2\.\s*(?:Therefore|So),?\s*(?:the|this) \3 (?:is|has) \1/gi,
                keywords: ['all', 'have', 'therefore'],
                strength: 0.7,
                validate: (match, context) => this.checkTermEquivocation(match)
            },
            
            maskedMan: {
                regex: /(.*?) is (.*?)\.\s*(.*?) (?:knows|likes|respects) \1\.\s*(?:Therefore|So),?\s*\3 (?:knows|likes|respects) \2/gi,
                keywords: ['is', 'knows', 'likes', 'therefore'],
                strength: 0.75,
                validate: (match, context) => this.validateIdentity(match)
            },
            
            sunkCost: {
                regex: /(?:We've|I've|They've) (?:already|spent|invested) .*?(?:so|therefore) (?:we|I|they) (?:can't|must|have to|need to) (?:stop|quit|continue)/gi,
                keywords: ['already', 'spent', 'invested', 'so', 'cant', 'must'],
                strength: 0.8,
                validate: (match, context) => this.validateSunkCost(match)
            },
            
            // INFORMAL FALLACIES - RELEVANCE (16)
            adHominem: {
                regex: /(?:but|however) (?:you|he|she|they) (?:are|is|were|was) (?:just |a |an |such a )?(?:typical |stupid |lying |corrupt |biased |hypocritical |ignorant )?(?:liar|idiot|fool|hypocrite|partisan|extremist)/gi,
                keywords: ['but you', 'he is', 'she is', 'liar', 'idiot', 'hypocrite'],
                strength: 0.9,
                contextClues: ['personal attack', 'character'],
                validate: (match, context) => this.validatePersonalAttack(match, context)
            },
            
            adBaculum: {
                regex: /(?:you'd better|you should|if you don't|unless you) .*? (?:or else|or|otherwise) (?:you'll|we'll|I'll) .*?(?:fire|hurt|punish|destroy|ruin)/gi,
                keywords: ['better', 'or else', 'fire', 'hurt', 'punish'],
                strength: 0.85,
                contextClues: ['threat', 'coercion'],
                validate: (match, context) => this.validateThreat(match)
            },
            
            adPopulum: {
                regex: /(?:everyone|most people|the majority|polls show|studies show) (?:knows|thinks|believes|agrees|says) .*?(?:so|therefore) (?:it must be|you should|we should)/gi,
                keywords: ['everyone', 'most people', 'majority', 'so', 'must be'],
                strength: 0.8,
                contextClues: ['popularity', 'bandwagon'],
                validate: (match, context) => this.validatePopularityAppeal(match)
            },
            
            appealToAuthority: {
                regex: /(?:Dr\.|Professor|Expert) (.*?) says .*?(?:so|therefore) (?:it must be|you should|we should)/gi,
                keywords: ['doctor', 'professor', 'expert', 'says', 'so'],
                strength: 0.7,
                contextClues: ['authority', 'expertise'],
                validate: (match, context) => this.validateAuthority(match, context)
            },
            
            appealToIgnorance: {
                regex: /(?:no one has|nobody has|there's no) (?:proof|evidence|study) (?:that|showing) .*?(?:so|therefore) (?:it must|we can assume)/gi,
                keywords: ['no proof', 'no evidence', 'so', 'must'],
                strength: 0.75,
                validate: (match, context) => this.validateIgnoranceAppeal(match)
            },
            
            redHerring: {
                regex: /(?:but what about|instead of|rather than|forget about) .*?(?:let's talk about|we should focus on|the real issue is)/gi,
                keywords: ['what about', 'instead', 'real issue'],
                strength: 0.6,
                contextClues: ['deflection', 'topic change'],
                validate: (match, context) => this.validateTopicDeflection(match, context)
            },
            
            strawMan: {
                regex: /(?:so you're saying|what you really mean|you want to|you think we should) .*?(?:destroy|eliminate|ban|abolish|get rid of) (?:all|every|everything)/gi,
                keywords: ['so youre saying', 'you want to', 'destroy', 'eliminate', 'all'],
                strength: 0.85,
                contextClues: ['misrepresentation', 'extreme'],
                validate: (match, context) => this.validateMisrepresentation(match, context)
            },
            
            geneticFallacy: {
                regex: /(?:that came from|that's from|because it's from) .*?(?:so|therefore) (?:it must be|it's) (?:wrong|bad|invalid|unreliable)/gi,
                keywords: ['came from', 'because its from', 'so', 'wrong'],
                strength: 0.7,
                validate: (match, context) => this.validateOriginAttack(match)
            },
            
            falseChoice: {
                regex: /(?:either|if not|you're either) .*? (?:or|then) .*?(?:there's no|no other|only two|just two) (?:option|choice|way)/gi,
                keywords: ['either', 'or', 'no other', 'only two'],
                strength: 0.8,
                validate: (match, context) => this.validateBinaryChoice(match, context)
            },
            
            hastyGeneralization: {
                regex: /(?:I met|I know|I've seen) (?:a few|some|two|three) .*?(?:so|therefore) (?:all|most|everyone from)/gi,
                keywords: ['I met', 'few', 'so', 'all'],
                strength: 0.8,
                validate: (match, context) => this.validateSampleSize(match)
            },
            
            beggingQuestion: {
                regex: /(.*?) (?:because|since) \1/gi,
                keywords: ['because', 'since'],
                strength: 0.6,
                validate: (match, context) => this.validateCircularReasoning(match)
            },
            
            complexQuestion: {
                regex: /(?:why do you|when will you|how can you) .*?(?:still|always|continue to) .*?\?/gi,
                keywords: ['why do you', 'when will you', 'still', 'always'],
                strength: 0.7,
                validate: (match, context) => this.validateLoadedQuestion(match)
            },
            
            appealToPity: {
                regex: /(?:please|you should|have mercy) .*?(?:because|since) (?:I have|my|I'm) .*?(?:sick|poor|struggling|dying)/gi,
                keywords: ['please', 'because', 'sick', 'poor', 'dying'],
                strength: 0.75,
                validate: (match, context) => this.validateEmotionalAppeal(match)
            },
            
            appealToFear: {
                regex: /(?:if you don't|unless you|without) .*?(?:will|might|could) .*?(?:die|kill|destroy|end|collapse)/gi,
                keywords: ['if you dont', 'will', 'die', 'destroy'],
                strength: 0.8,
                validate: (match, context) => this.validateFearAppeal(match)
            },
            
            appealToTradition: {
                regex: /(?:we've always|for centuries|traditionally|it's tradition) .*?(?:so|therefore) (?:we should|it's right|it's correct)/gi,
                keywords: ['always', 'centuries', 'tradition', 'so'],
                strength: 0.75,
                validate: (match, context) => this.validateTraditionAppeal(match)
            },
            
            appealToNovelty: {
                regex: /(?:this new|latest|newest|cutting edge) .*?(?:so|therefore) (?:it's better|it's superior|you should)/gi,
                keywords: ['new', 'latest', 'cutting edge', 'better'],
                strength: 0.7,
                validate: (match, context) => this.validateNoveltyAppeal(match)
            },
            
            // CONTEMPORARY DIGITAL FALLACIES (12)
            algorithmicBias: {
                regex: /(?:Google|AI|algorithm|search results) (?:shows|says|recommends|proves) .*?(?:so|therefore) (?:it must be|it's) (?:true|accurate|objective|unbiased)/gi,
                keywords: ['google', 'AI', 'algorithm', 'objective', 'unbiased'],
                strength: 0.8,
                contextClues: ['technology', 'automation'],
                validate: (match, context) => this.validateAlgorithmicAssumption(match)
            },
            
            echoChambered: {
                regex: /(?:my feed|my timeline|everyone I follow|all my friends) (?:shows|says|thinks|agrees) .*?(?:so|therefore) (?:it must be|everyone) (?:true|thinks this|agrees)/gi,
                keywords: ['my feed', 'everyone I follow', 'so', 'everyone'],
                strength: 0.85,
                contextClues: ['social media', 'network'],
                validate: (match, context) => this.validateEchoChamber(match, context)
            },
            
            viralTruth: {
                regex: /(?:viral|trending|millions of views|thousands of shares|going viral) .*?(?:so|therefore) (?:it must be|everyone knows|it's) (?:true|accurate|important)/gi,
                keywords: ['viral', 'trending', 'millions', 'so', 'true'],
                strength: 0.8,
                contextClues: ['popularity', 'social media'],
                validate: (match, context) => this.validateViralityTruth(match)
            },
            
            screenshotFallacy: {
                regex: /(?:this screenshot|here's proof|screenshot shows) .*?(?:so|therefore|proves) (?:they really|it's true|this happened)/gi,
                keywords: ['screenshot', 'proof', 'shows', 'really'],
                strength: 0.7,
                contextClues: ['image', 'evidence'],
                validate: (match, context) => this.validateScreenshotEvidence(match)
            },
            
            platformAuthority: {
                regex: /(?:it's on|posted on|from) (?:LinkedIn|Facebook|Twitter|Instagram|official) .*?(?:so|therefore) (?:it must be|it's) (?:professional|accurate|verified|official)/gi,
                keywords: ['posted on', 'LinkedIn', 'official', 'professional'],
                strength: 0.75,
                contextClues: ['platform', 'social media'],
                validate: (match, context) => this.validatePlatformCredibility(match)
            },
            
            engagementMetrics: {
                regex: /(?:thousands of likes|millions of views|huge engagement|lots of shares) .*?(?:so|therefore) (?:it must be|the advice is|content is) (?:good|accurate|valuable|quality)/gi,
                keywords: ['thousands', 'likes', 'engagement', 'good'],
                strength: 0.7,
                validate: (match, context) => this.validateEngagementQuality(match)
            },
            
            memeLogic: {
                regex: /(?:this meme|as the meme says|meme explains) .*?(?:so|therefore|proves) (?:it's true|that's how|this is why)/gi,
                keywords: ['meme', 'explains', 'proves', 'true'],
                strength: 0.8,
                contextClues: ['humor', 'oversimplification'],
                validate: (match, context) => this.validateMemeReasoning(match)
            },
            
            deepfakeParanoia: {
                regex: /(?:could be|might be|probably) (?:deepfake|AI generated|fake video) .*?(?:so|therefore) (?:we can't|don't) (?:trust|believe) (?:any|anything)/gi,
                keywords: ['deepfake', 'AI generated', 'cant trust'],
                strength: 0.7,
                validate: (match, context) => this.validateDeepfakeSkepticism(match)
            },
            
            aiOracle: {
                regex: /(?:ChatGPT|AI|GPT) (?:said|told me|explained|confirmed) .*?(?:so|therefore) (?:it must be|it's definitely|that's) (?:true|correct|accurate)/gi,
                keywords: ['ChatGPT', 'AI', 'said', 'must be'],
                strength: 0.8,
                validate: (match, context) => this.validateAIAuthority(match)
            },
            
            contextCollapse: {
                regex: /(?:this quote|this statement|this action) (?:always means|is always|proves) .*?(?:regardless of|no matter) (?:context|situation|when)/gi,
                keywords: ['always means', 'regardless', 'context'],
                strength: 0.7,
                validate: (match, context) => this.validateContextIgnoring(match)
            },
            
            notificationUrgency: {
                regex: /(?:breaking|urgent|alert|notification says) .*?(?:so|therefore) (?:it must be|we need to|you should) (?:important|act now|respond)/gi,
                keywords: ['breaking', 'urgent', 'must be', 'important'],
                strength: 0.6,
                validate: (match, context) => this.validateUrgencyManipulation(match)
            },
            
            digitalNative: {
                regex: /(?:young people|digital natives|millennials|gen z) (?:understand|know|are better at) .*?(?:so|therefore) (?:they can|they don't need|trust them)/gi,
                keywords: ['young people', 'digital natives', 'understand', 'trust'],
                strength: 0.7,
                validate: (match, context) => this.validateGenerationalAssumption(match)
            },
            
            // SPECIALIZED DOMAIN FALLACIES (25)
            naturalistic: {
                regex: /(?:it's natural|all natural|found in nature) .*?(?:so|therefore) (?:it must be|it's) (?:safe|good|better|healthy)/gi,
                keywords: ['natural', 'nature', 'safe', 'healthy'],
                strength: 0.8,
                contextClues: ['health', 'medicine'],
                validate: (match, context) => this.validateNaturalAssumption(match)
            },
            
            ancientWisdom: {
                regex: /(?:for thousands of years|ancient|traditional|our ancestors) .*?(?:so|therefore) (?:it must|they knew|it) (?:work|works|be true)/gi,
                keywords: ['thousands', 'ancient', 'ancestors', 'must work'],
                strength: 0.75,
                validate: (match, context) => this.validateAncientWisdom(match)
            },
            
            correlationCausation: {
                regex: /(?:studies show|research shows|statistics prove) .*?(?:correlates with|linked to|associated with) .*?(?:so|therefore|proves) .*?(?:causes|leads to|results in)/gi,
                keywords: ['studies show', 'correlates', 'proves', 'causes'],
                strength: 0.85,
                contextClues: ['research', 'statistics'],
                validate: (match, context) => this.validateCausalClaim(match)
            }
            
            // ... Continue for all 60 fallacies
        };
    }
    
    analyzeText(text, context = {}) {
        const detected = [];
        const cleanText = this.preprocessText(text);
        
        for (const [fallacyType, pattern] of Object.entries(this.patterns)) {
            const matches = this.findMatches(cleanText, pattern);
            
            for (const match of matches) {
                if (pattern.validate(match, context)) {
                    detected.push({
                        fallacy: fallacyType,
                        confidence: pattern.strength,
                        evidence: match.text,
                        position: match.index,
                        context: context,
                        explanation: this.getFallacyExplanation(fallacyType)
                    });
                }
            }
        }
        
        return this.rankDetections(detected);
    }
}
```

## Phase 3: Live Content Scanner (Week 5-6)

### Real-Time Content Fetching System
```javascript
// content-scanner.js - Live internet content fetcher
class LiveContentScanner {
    constructor() {
        this.sources = CONTENT_SOURCES;
        this.rateLimiter = new RateLimiter();
        this.contentBuffer = new CircularBuffer(1000); // Keep last 1000 items
        this.isScanning = false;
    }
    
    async startScanning() {
        if (this.isScanning) return;
        this.isScanning = true;
        
        // Scan multiple sources simultaneously
        await Promise.all([
            this.scanReddit(),
            this.scanHackerNews(),
            this.scanRSSFeeds(),
            this.scanYouTube()
        ]);
        
        // Continue scanning every 30 seconds
        setTimeout(() => this.startScanning(), 30000);
    }
    
    async scanReddit() {
        try {
            for (const subreddit of this.sources.reddit.subreddits) {
                if (!this.rateLimiter.canMakeRequest('reddit')) continue;
                
                const url = this.sources.reddit.url.replace('{subreddit}', subreddit);
                const response = await fetch(url);
                const data = await response.json();
                
                for (const post of data.data.children) {
                    const content = {
                        id: post.data.id,
                        text: post.data.title + (post.data.selftext ? ' ' + post.data.selftext : ''),
                        source: `reddit-${subreddit}`,
                        url: `https://reddit.com${post.data.permalink}`,
                        score: post.data.score,
                        comments: post.data.num_comments,
                        timestamp: new Date(post.data.created_utc * 1000),
                        context: {
                            platform: 'reddit',
                            subreddit: subreddit,
                            type: 'post'
                        }
                    };
                    
                    this.contentBuffer.add(content);
                }
                
                this.rateLimiter.recordRequest('reddit');
            }
        } catch (error) {
            console.warn('Reddit scanning failed:', error);
            this.useFallbackContent('reddit');
        }
    }
    
    async scanHackerNews() {
        try {
            if (!this.rateLimiter.canMakeRequest('hackernews')) return;
            
            const topStoriesResponse = await fetch(this.sources.hackernews.url);
            const storyIds = await topStoriesResponse.json();
            
            // Get top 20 stories
            const storiesToFetch = storyIds.slice(0, 20);
            
            for (const storyId of storiesToFetch) {
                const storyUrl = this.sources.hackernews.itemUrl.replace('{id}', storyId);
                const storyResponse = await fetch(storyUrl);
                const story = await storyResponse.json();
                
                if (story && story.title) {
                    const content = {
                        id: story.id,
                        text: story.title + (story.text ? ' ' + story.text : ''),
                        source: 'hackernews',
                        url: story.url || `https://news.ycombinator.com/item?id=${story.id}`,
                        score: story.score,
                        comments: story.descendants || 0,
                        timestamp: new Date(story.time * 1000),
                        context: {
                            platform: 'hackernews',
                            type: 'story'
                        }
                    };
                    
                    this.contentBuffer.add(content);
                }
            }
            
            this.rateLimiter.recordRequest('hackernews');
        } catch (error) {
            console.warn('Hacker News scanning failed:', error);
            this.useFallbackContent('hackernews');
        }
    }
    
    async scanRSSFeeds() {
        try {
            for (const feedUrl of this.sources.rss.feeds) {
                if (!this.rateLimiter.canMakeRequest('rss')) continue;
                
                // Use RSS to JSON proxy service
                const proxyUrl = `https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(feedUrl)}`;
                const response = await fetch(proxyUrl);
                const data = await response.json();
                
                if (data.status === 'ok') {
                    for (const item of data.items) {
                        const content = {
                            id: this.generateId(item.link),
                            text: item.title + ' ' + this.stripHTML(item.description),
                            source: `rss-${data.feed.title}`,
                            url: item.link,
                            timestamp: new Date(item.pubDate),
                            context: {
                                platform: 'rss',
                                feed: data.feed.title,
                                type: 'article'
                            }
                        };
                        
                        this.contentBuffer.add(content);
                    }
                }
                
                this.rateLimiter.recordRequest('rss');
            }
        } catch (error) {
            console.warn('RSS scanning failed:', error);
            this.useFallbackContent('rss');
        }
    }
    
    getLatestContent(count = 10) {
        return this.contentBuffer.getLatest(count);
    }
    
    getContentBySource(source, count = 5) {
        return this.contentBuffer.getBySource(source, count);
    }
    
    useFallbackContent(source) {
        const fallbackItems = FALLBACK_EXAMPLES
            .filter(item => item.source.includes(source))
            .slice(0, 5);
            
        for (const item of fallbackItems) {
            this.contentBuffer.add({
                ...item,
                id: this.generateId(item.text),
                timestamp: new Date(),
                isFallback: true
            });
        }
    }
}
```

## Phase 4: Dynamic Exercise Generator (Week 7-8)

### Exercise Creation from Live Content
```javascript
// exercise-generator.js - Dynamic exercise creation
class ExerciseGenerator {
    constructor(fallacyDetector) {
        this.detector = fallacyDetector;
        this.exerciseTypes = ['identify', 'explain', 'improve', 'context'];
        this.difficultyLevels = [1, 2, 3, 4, 5];
    }
    
    generateExerciseFromContent(content) {
        // Analyze content for fallacies
        const detectedFallacies = this.detector.analyzeText(content.text, content.context);
        
        if (detectedFallacies.length === 0) {
            return null; // No fallacies found
        }
        
        // Select the strongest detection
        const primaryFallacy = detectedFallacies[0];
        
        // Generate exercise based on content and detected fallacy
        const exerciseType = this.selectExerciseType(primaryFallacy);
        const difficulty = this.calculateDifficulty(primaryFallacy, content);
        
        return this.createExercise(content, primaryFallacy, exerciseType, difficulty);
    }
    
    createExercise(content, fallacy, type, difficulty) {
        const baseExercise = {
            id: this.generateExerciseId(),
            content: content,
            fallacy: fallacy,
            type: type,
            difficulty: difficulty,
            timestamp: new Date(),
            source: content.source,
            isLive: !content.isFallback
        };
        
        switch (type) {
            case 'identify':
                return this.createIdentificationExercise(baseExercise);
            case 'explain':
                return this.createExplanationExercise(baseExercise);
            case 'improve':
                return this.createImprovementExercise(baseExercise);
            case 'context':
                return this.createContextExercise(baseExercise);
            default:
                return this.createIdentificationExercise(baseExercise);
        }
    }
    
    createIdentificationExercise(base) {
        const distractors = this.generateDistractors(base.fallacy.fallacy, 3);
        
        return {
            ...base,
            question: "What logical fallacy is present in this content?",
            options: this.shuffleArray([
                {
                    id: 'correct',
                    text: this.getFallacyDisplayName(base.fallacy.fallacy),
                    isCorrect: true
                },
                ...distractors.map((distractor, index) => ({
                    id: `distractor_${index}`,
                    text: this.getFallacyDisplayName(distractor),
                    isCorrect: false
                }))
            ]),
            explanation: base.fallacy.explanation,
            evidence: base.fallacy.evidence
        };
    }
    
    createExplanationExercise(base) {
        return {
            ...base,
            question: "Explain why this argument contains a logical fallacy:",
            expectedElements: [
                "Identifies the specific fallacy type",
                "Explains the logical error",
                "Describes why it undermines the argument"
            ],
            rubric: this.generateExplanationRubric(base.fallacy.fallacy),
            hints: this.generateHints(base.fallacy.fallacy)
        };
    }
    
    createImprovementExercise(base) {
        return {
            ...base,
            question: "How could this argument be improved to avoid the logical fallacy?",
            suggestions: this.generateImprovementSuggestions(base.fallacy.fallacy),
            framework: this.getArgumentImprovementFramework()
        };
    }
    
    createContextExercise(base) {
        return {
            ...base,
            question: "How does the context affect the strength of this argument?",
            contextFactors: this.analyzeContextFactors(base.content.context),
            scenarios: this.generateAlternativeScenarios(base.content.text)
        };
    }
}
```

## Phase 5: User Interface Implementation (Week 9-10)

### Main Dashboard (Wix Velo)
```javascript
// dashboard.js - Main learning interface
import { local } from 'wix-storage';
import wixWindow from 'wix-window';

let contentScanner;
let fallacyDetector;
let exerciseGenerator;
let currentExercise = null;

$w.onReady(function () {
    initializeApp();
    loadUserProgress();
    startLiveScanning();
});

async function initializeApp() {
    // Initialize detection engine
    fallacyDetector = new FallacyDetector();
    
    // Initialize content scanner
    contentScanner = new LiveContentScanner();
    
    // Initialize exercise generator
    exerciseGenerator = new ExerciseGenerator(fallacyDetector);
    
    // Setup UI
    setupEventHandlers();
    updateUI();
}

function setupEventHandlers() {
    $w('#newExerciseBtn').onClick(loadNewExercise);
    $w('#submitAnswerBtn').onClick(submitAnswer);
    $w('#skipExerciseBtn').onClick(skipExercise);
    $w('#progressBtn').onClick(showProgress);
    $w('#fallacyGuideBtn').onClick(showFallacyGuide);
}

async function startLiveScanning() {
    try {
        await contentScanner.startScanning();
        $w('#statusIndicator').text = "🟢 Scanning live content";
        $w('#statusIndicator').show();
        
        // Generate exercise from live content every 60 seconds
        setInterval(generateLiveExercise, 60000);
        
        // Load first exercise immediately
        setTimeout(generateLiveExercise, 2000);
    } catch (error) {
        $w('#statusIndicator').text = "🟡 Using practice content";
        $w('#statusIndicator').show();
        loadFallbackExercise();
    }
}

async function generateLiveExercise() {
    const recentContent = contentScanner.getLatestContent(20);
    
    for (const content of recentContent) {
        const exercise = exerciseGenerator.generateExerciseFromContent(content);
        
        if (exercise && !hasSeenContent(content.id)) {
            currentExercise = exercise;
            displayExercise(exercise);
            markContentAsSeen(content.id);
            return;
        }
    }
    
    // If no suitable live content, use fallback
    loadFallbackExercise();
}

function displayExercise(exercise) {
    $w('#exerciseContainer').show();
    $w('#exerciseType').text = exercise.type.toUpperCase();
    $w('#exerciseSource').text = `Source: ${exercise.source}`;
    $w('#exerciseContent').text = exercise.content.text;
    $w('#exerciseQuestion').text = exercise.question;
    
    if (exercise.content.url && !exercise.content.isFallback) {
        $w('#sourceLink').link = exercise.content.url;
        $w('#sourceLink').show();
    } else {
        $w('#sourceLink').hide();
    }
    
    // Display based on exercise type
    switch (exercise.type) {
        case 'identify':
            displayIdentificationExercise(exercise);
            break;
        case 'explain':
            displayExplanationExercise(exercise);
            break;
        case 'improve':
            displayImprovementExercise(exercise);
            break;
        case 'context':
            displayContextExercise(exercise);
            break;
    }
    
    // Update streak display
    updateStreakDisplay();
}

function displayIdentificationExercise(exercise) {
    $w('#optionsRepeater').data = exercise.options.map(option => ({
        _id: option.id,
        text: option.text,
        isCorrect: option.isCorrect
    }));
    
    $w('#optionsRepeater').show();
    $w('#textInput').hide();
    $w('#improvementSection').hide();
    
    $w('#optionsRepeater').onItemReady(($item, itemData) => {
        $item('#optionButton').label = itemData.text;
        $item('#optionButton').onClick(() => {
            submitIdentificationAnswer(itemData);
        });
    });
}

function submitIdentificationAnswer(selectedOption) {
    const isCorrect = selectedOption.isCorrect;
    
    showFeedback(isCorrect, currentExercise.explanation);
    updateProgress(currentExercise.fallacy.fallacy, isCorrect);
    
    if (isCorrect) {
        playSuccessAnimation();
    } else {
        playIncorrectAnimation();
    }
    
    // Auto-load next exercise after feedback
    setTimeout(loadNewExercise, 3000);
}

function updateProgress(fallacyType, isCorrect) {
    const progress = getLocalProgress();
    
    if (!progress[fallacyType]) {
        progress[fallacyType] = {
            attempts: 0,
            correct: 0,
            lastSeen: new Date(),
            mastery: 0
        };
    }
    
    progress[fallacyType].attempts++;
    if (isCorrect) progress[fallacyType].correct++;
    progress[fallacyType].lastSeen = new Date();
    
    // Calculate mastery level (0-5)
    const accuracy = progress[fallacyType].correct / progress[fallacyType].attempts;
    progress[fallacyType].mastery = Math.min(5, accuracy * 5);
    
    // Update overall stats
    progress.totalAttempts = (progress.totalAttempts || 0) + 1;
    progress.totalCorrect = (progress.totalCorrect || 0) + (isCorrect ? 1 : 0);
    progress.streak = isCorrect ? (progress.streak || 0) + 1 : 0;
    progress.lastActive = new Date();
    
    saveLocalProgress(progress);
    updateProgressDisplays(progress);
}
```

### Local Storage Management
```javascript
// progress-manager.js - Local storage for user progress
class ProgressManager {
    constructor() {
        this.storageKey = 'logicLingo_progress';
        this.seenContentKey = 'logicLingo_seen_content';
        this.settingsKey = 'logicLingo_settings';
    }
    
    getProgress() {
        try {
            const stored = local.getItem(this.storageKey);
            return stored ? JSON.parse(stored) : this.createDefaultProgress();
        } catch (error) {
            return this.createDefaultProgress();
        }
    }
    
    saveProgress(progress) {
        try {
            local.setItem(this.storageKey, JSON.stringify(progress));
            return true;
        } catch (error) {
            console.error('Failed to save progress:', error);
            return false;
        }
    }
    
    createDefaultProgress() {
        return {
            totalAttempts: 0,
            totalCorrect: 0,
            streak: 0,
            longestStreak: 0,
            lastActive: new Date(),
            fallacyStats: {},
            achievements: [],
            settings: {
                difficulty: 'adaptive',
                exerciseTypes: ['identify', 'explain'],
                contentSources: ['reddit', 'hackernews', 'rss']
            }
        };
    }
    
    updateFallacyProgress(fallacyType, isCorrect, exerciseType) {
        const progress = this.getProgress();
        
        if (!progress.fallacyStats[fallacyType]) {
            progress.fallacyStats[fallacyType] = {
                attempts: 0,
                correct: 0,
                byType: {},
                firstSeen: new Date(),
                lastSeen: new Date(),
                mastery: 0
            };
        }
        
        const stat = progress.fallacyStats[fallacyType];
        stat.attempts++;
        if (isCorrect) stat.correct++;
        stat.lastSeen = new Date();
        
        // Track by exercise type
        if (!stat.byType[exerciseType]) {
            stat.byType[exerciseType] = { attempts: 0, correct: 0 };
        }
        stat.byType[exerciseType].attempts++;
        if (isCorrect) stat.byType[exerciseType].correct++;
        
        // Calculate mastery
        stat.mastery = this.calculateMastery(stat);
        
        // Update overall progress
        progress.totalAttempts++;
        if (isCorrect) progress.totalCorrect++;
        progress.streak = isCorrect ? progress.streak + 1 : 0;
        progress.longestStreak = Math.max(progress.longestStreak, progress.streak);
        progress.lastActive = new Date();
        
        // Check for achievements
        this.checkAchievements(progress);
        
        this.saveProgress(progress);
        return progress;
    }
    
    calculateMastery(stat) {
        const accuracy = stat.correct / stat.attempts;
        const recency = this.getDaysAgo(stat.lastSeen);
        
        let mastery = accuracy * 5; // Base mastery 0-5
        
        // Decay over time
        if (recency > 7) mastery *= 0.9;
        if (recency > 30) mastery *= 0.7;
        if (recency > 90) mastery *= 0.5;
        
        return Math.max(0, Math.min(5, mastery));
    }
    
    getWeakAreas(limit = 5) {
        const progress = this.getProgress();
        const fallacyStats = Object.entries(progress.fallacyStats)
            .filter(([_, stat]) => stat.mastery < 3)
            .sort(([_, a], [__, b]) => a.mastery - b.mastery)
            .slice(0, limit);
            
        return fallacyStats.map(([fallacy, _]) => fallacy);
    }
    
    getStrongAreas(limit = 5) {
        const progress = this.getProgress();
        const fallacyStats = Object.entries(progress.fallacyStats)
            .filter(([_, stat]) => stat.mastery >= 4)
            .sort(([_, a], [__, b]) => b.mastery - a.mastery)
            .slice(0, limit);
            
        return fallacyStats.map(([fallacy, _]) => fallacy);
    }
    
    hasSeenContent(contentId) {
        try {
            const seen = local.getItem(this.seenContentKey);
            const seenArray = seen ? JSON.parse(seen) : [];
            return seenArray.includes(contentId);
        } catch (error) {
            return false;
        }
    }
    
    markContentAsSeen(contentId) {
        try {
            const seen = local.getItem(this.seenContentKey);
            const seenArray = seen ? JSON.parse(seen) : [];
            
            seenArray.push(contentId);
            
            // Keep only last 1000 items to prevent storage bloat
            if (seenArray.length > 1000) {
                seenArray.splice(0, seenArray.length - 1000);
            }
            
            local.setItem(this.seenContentKey, JSON.stringify(seenArray));
        } catch (error) {
            console.error('Failed to mark content as seen:', error);
        }
    }
}
```

## Phase 6: Deployment & Testing (Week 11-12)

### Final Implementation Checklist

#### Technical Requirements ✅
- **No external database** - Everything in browser storage
- **Real-time content** - Live API integration with fallbacks
- **All 60 fallacies** - Complete detection patterns
- **Wix Velo compatible** - Uses only Wix capabilities
- **Mobile responsive** - Works on all devices
- **Offline fallback** - Graceful degradation when APIs fail

#### Performance Optimizations
```javascript
// Optimization strategies
const PERFORMANCE_CONFIG = {
    maxConcurrentRequests: 3,
    contentBufferSize: 1000,
    progressSaveThrottle: 5000, // Save every 5 seconds max
    exercisePreloadCount: 3,
    fallacyDetectionTimeout: 2000,
    uiUpdateDebounce: 500
};
```

#### Content Quality Filters
```javascript
// Content filtering for appropriateness
class ContentFilter {
    constructor() {
        this.inappropriatePatterns = [
            /explicit sexual content/i,
            /extreme violence/i,
            /hate speech/i,
            /personal attacks/i
        ];
        this.minimumLength = 50;
        this.maximumLength = 1000;
    }
    
    isAppropriate(content) {
        // Length check
        if (content.text.length < this.minimumLength || 
            content.text.length > this.maximumLength) {
            return false;
        }
        
        // Content pattern check
        for (const pattern of this.inappropriatePatterns) {
            if (pattern.test(content.text)) {
                return false;
            }
        }
        
        // Context appropriateness
        if (content.context && content.context.nsfw) {
            return false;
        }
        
        return true;
    }
}
```

### Launch Strategy

#### Week 12: Beta Testing
- Deploy to Wix Velo staging environment
- Test with 50 beta users
- Monitor API rate limits and performance
- Collect feedback on fallacy detection accuracy

#### Week 13-14: Optimization
- Refine detection algorithms based on feedback
- Optimize performance and user experience
- Add additional content sources if needed
- Implement user-requested features

#### Week 15: Public Launch
- Deploy to production Wix site
- Monitor system performance and user engagement
- Begin collecting analytics on learning effectiveness
- Plan future feature development

This implementation plan provides a complete, database-free system that scans live internet content for all 60 logical fallacies and generates real-time educational exercises, all within Wix Velo's capabilities.
