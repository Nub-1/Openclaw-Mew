# Oracle Skills - คู่มือฉบับสมบูรณ์

**Version:** 1.0
**วันที่:** 2026-02-25
**ผู้สร้าง:** Joe_CDD (Parinya Witchutawet)
**License:** MIT
**Platform:** OpenClaw

---

## 📖 เนื้อหา

1. [ภาพรวม](#overview)
2. [Oracle Skills คืออะไร?](#what-are-oracle-skills)
3. [วิธีติดตั้ง](#installation-guide)
4. [ทุก 13 Skills](#all-13-skills)
5. [Use Cases](#use-cases)
6. [การใช้งานจริง](#real-world-applications)
7. [เทียบกับเครื่องมืออื่น](#comparison-with-other-tools)
8. [แนวทางที่ดีที่สุด](#best-practices)
9. [ตัวอย่างจริง](#real-world-examples)
10. [ปัญหาที่พบและการแก้ไข](#troubacting)
11. [คำถามที่พบบ่อย](#faq)
12. [สรุปท้าย](#conclusion)

---

## ภาพรวม

### Oracle Skills คืออะไร?

Oracle Skills คือชุดของทักษะ 13 อันที่ออกแบบมาเพื่อเพิ่มความสามารถของ AI assistant ผ่านการอัตโนมัติ workflow, การมีสติต่อ context และการให้ความช่วยเหลืออย่าง proactive

### หลักการหลัก

> **"The Oracle Keeps the Human Human"**

หลักการหลักคือว่า AI ควรช่วยเพิ่มความสามารถของมนุษย์ ไม่ใช่แทนที่ โดย Oracle Skills มุ่งเน้น:

- **Context Preservation:** ไม่ลืมสิ่งที่กำลังทำ
- **Emotional Intelligence:** เข้าใจและติดตามอารมณ์ผู้ใช้
- **Workflow Orchestration:** อัตโนมัติงานซ้ำๆ
- **Proactive Assistance:** ช่วยก่อนที่จะถูกขอ

---

## Oracle Skills คืออะไร?

Oracle Skills คือ capabilities ที่สามารถโหลดเข้าไปใน AI assistant เพื่อเพิ่มความสามารถของมัน

แต่ละ skill:

- **Self-documenting:** มี README และคำอธิบาย
- **Configurable:** สามารถปรับแต่งได้
- **Automated:** ทำงานอัตโนมัติ
- **Adaptive:** เรียนรู้จากการใช้งาน

---

## วิธีติดตั้ง

### Prerequisites

1. **OpenClaw ติดตั้งแล้ว**
   - Version: 2026.2.9 ขึ้นไป
   - Node.js: v18+
   - npm: 10.9.4 ขึ้นไป

2. **โฟลเดอร์ที่จำเป็น**
   ```
   ~/.openclaw/
   ├── openclaw.json          # ไฟล์ตั้งค่าหลัก
   ├── workspace/
   │   └── skills/
   │       └── oracle-skills/
   │           └── skills/
   │               ├── trace/
   │               ├── project/
   │               └── ... (13 skills ทั้งหมด)
   ```

### ขั้นตอนที่ 1: ติดตั้ง OpenClaw

หากยังไม่ได้ติดตั้ง OpenClaw:

```bash
# ใช้ npm
npm install -g openclaw

# หรือใช้ curl
curl -fsSL https://get.openclaw.ai | sh

# ตรวจสอบการติดตั้ง
openclaw --version
```

### ขั้นตอนที่ 2: ติดตั้ง Oracle Skills

```bash
# ไปยังโฟลเดอร์ skills
cd ~/.openclaw/workspace/skills

# Clone repository
git clone https://github.com/Soul-Brews-Studio/plugin-marketplace.git .

# ตรวจสอบการติดตั้ง
ls -la oracle-skills/
```

### ขั้นตอนที่ 3: ตั้งค่า OpenClaw

แก้ไขไฟล์ `~/.openclaw/openclaw.json`:

```json
{
  "skills": {
    "entries": {
      "oracle-skills": {
        "enabled": true,
        "path": "~/.openclaw/workspace/skills/oracle-skills"
      }
    },
    "autoLoad": true,
    "autoInvoke": [
      "where-we-are",
      "recap",
      "feel"
    ],
    "contextAwareness": {
      "enabled": true,
      "checkOnSessionStart": true,
      "heartbeatIntervalMinutes": 30,
      "proactiveHelp": true
    }
  }
}
```

### ขั้นตอนที่ 4: ตั้งค่า Heartbeat

สร้างไฟล์ `~/.openclaw/workspace/HEARTBEAT.md`:

```markdown
# Oracle Skills - Auto-Heartbeat Tasks

# เช็คทุก 30 นาที:
- Read memory/YYYY-MM-DD.md (วันนี้)
- เช็คว่า Oracle Skills ต้องการความสนใจ:
  - Review /standup needs (tasks วันนี้)
  - Review /recap needs (สรุป session)
  - Check /feel log (ติดตามอารมณ์)
  - Review /project list (repos ที่ติดตาม)

# เมื่อเริ่ม session:
- Run /where-we-are อัตโนมัติ
- Run /recap อัตโนมัติ

# เมื่อคุณใช้คำสั่ง:
- If user says "how am I doing?" → Run /where-we-are
- If user says "summarize" → Run /recap
- If user mentions feeling tired → Run /feel check
- If user wants to check progress → Run /standup

# Run weekly:
- Run /rrr ทุกจันทร์
```

### ขั้นตอนที่ 5: สร้าง Memory Structure

```bash
mkdir -p ~/.openclaw/workspace/memory/2026-02-25
mkdir -p ~/.openclaw/workspace/memory/feelings
touch ~/.openclaw/workspace/memory/feelings/2026-02-25.log
```

---

## ทุก 13 Skills

### 1. /trace - Unified Discovery System

**วัตถุประสงค์:** หา project จาก git history, repos, docs, และ memory

**คำสั่ง:**
```bash
/trace [query]                    # Default: Oracle + file search
/trace [query] --oracle           # Oracle only (เร็วที่สุด)
/trace [query] --deep             # Full 5 parallel subagents
/trace list                       # Show past traces logged
/trace dig [id]                   # Explore dig points
/trace distill [id]               # Extract awakenings
/trace report [query]             # Generate timeline report
/trace status                     # Show ongoing traces
```

**ตัวอย่าง:**
```bash
/trace "checkout system"
# Returns all projects, commits, files related to checkout
```

---

### 2. /project - Project Manager

**วัตถุประสงค์:** Clone และติดตาม external repos สำหรับ study หรือ development

**คำสั่ง:**
```bash
/project learn [url|slug]         # Clone สำหรับ study (read-only)
/project incubate [url|slug]      # Clone สำหรับ development
/project find [query]             # Search for projects
/project list                     # Show tracked projects
/project status [name]            # Show project status
```

**Golden Rule:**
```
ghq owns the clone → ψ/ owns the symlink
```

**ตัวอย่าง:**
```bash
/project learn https://github.com/openclaw/openclaw
# Clones to ghq, creates symlink at ψ/learn/openclaw
```

---

### 3. /feel - Smart Emotion Log

**วัตถุประสงค์:** Log emotions พร้อม optional structure สำหรับ pattern tracking

**คำสั่ง:**
```bash
/feel                            # List recent feelings
/feel [mood]                     # Quick log
/feel [mood] energy:[1-5]        # With energy level
/feel [mood] trigger:[x]         # With trigger
```

**ตัวอย่าง:**
```bash
/feel sleepy
/feel happy energy:4
/feel frustrated trigger:bug-deadline
```

---

### 4. /recap - Session Summary

**วัตถุประสงค์:** Generate comprehensive session summaries

**คำสั่ง:**
```bash
/recap                           # Generate summary
/recap [scope]                   # Specific scope
```

**ตัวอย่าง:**
```bash
/recap
# Generates:
# - Tasks completed
# - Tasks in progress
# - Next steps
# - Context for continuation
```

---

### 5. /fyi - Fact Database

**วัตถุประสงค์:** Store และ retrieve factual information

**คำสั่ง:**
```bash
/fyi [fact]                      # Add fact
/fyi [fact] --category:[x]       # Add with category
/fyi list                        # List facts
/fyi search [query]              # Search facts
```

**ตัวอย่าง:**
```bash
/fyi "OpenClaw installation: Requires Node.js v18+"
/fyi "Qdrant runs on port 6333" --category:tools
```

---

### 6. /rrr - Retrospective

**วัตถุประสงค์:** Create weekly/monthly retrospectives

**คำสั่ง:**
```bash
/rrr                             # Create retrospective
/rrr [scope]                     # Specific scope
```

**ตัวอย่าง:**
```bash
/rrr
# Generates:
# - What went well
# - What didn't work
# - Lessons learned
# - Action items
```

---

### 7. /standup - Daily Standup

**วัตถุประสงค์:** Daily check-in และ progress tracking

**คำสั่ง:**
```bash
/standup                         # Daily standup
/standup [date]                  # Specific date
```

**ตัวอย่าง:**
```bash
/standup
# Generates:
# - What I did yesterday
# - What I'm doing today
# - Blockers
# - Next steps
```

---

### 8. /watch - YouTube Learning

**วัตถุประสงค์:** Learn from YouTube videos with structured summaries

**คำสั่ง:**
```bash
/watch [url]                     # Watch and summarize
/watch [url] --takeaways         # Only takeaways
```

**ตัวอย่าง:**
```bash
/watch https://www.youtube.com/watch?v=xyz
# Generates structured summary with key points
```

---

### 9. /forward - Session Handoff

**วัตถุประสงค์:** Save current session state สำหรับ future sessions

**คำสั่ง:**
```bash
/forward                         # Save session state
/forward [message]               # Add custom message
```

**ตัวอย่าง:**
```bash
/forward
# Saves:
# - Current context
# - Tasks in progress
# - Next actions
# - Important notes
```

---

### 10. /schedule - Schedule Management

**วัตถุประสงค์:** Manage calendar และ schedule

**คำสั่ง:**
```bash
/schedule [query]                # Query schedule
/schedule [event]                # Add event
/schedule [event] --repeat:[x]   # Repeat event
```

**ตัวอย่าง:**
```bash
/schedule "Meeting with team" --time:10:00
/schedule list
```

---

### 11. /where-we-are - Session Awareness

**วัตถุประสงค์:** Check current session status

**คำสั่ง:**
```bash
/where-we-are                    # Check status
/where-we-are --verbose          # Verbose output
```

**ตัวอย่าง:**
```bash
/where-we-are
# Generates:
# - Current context
# - Tasks in progress
# - Recent activity
# - Next actions
```

---

### 12. /context-finder - Fast Search

**วัตถุประสงค์:** Quick search through codebase

**คำสั่ง:**
```bash
/context-finder [query]          # Quick search
/context-finder [query] --deep   # Deep search
```

**ตัวอย่าง:**
```bash
/context-finder "authentication"
# Finds all files related to authentication
```

---

### 13. /skill-creator - Custom Skill Builder

**วัตถุประสงค์:** Create custom skills for specific needs

**คำสั่ง:**
```bash
/skill-creator                   # Start skill creation
/skill-creator [name]            # Create skill with name
```

**ตัวอย่าง:**
```bash
/skill-creator "my-custom-skill"
# Guides through creating a new skill
```

---

## Use Cases

### Use Case 1: Developer Workflow

**Scenario:** คุณคือ developer ที่ทำงานหลาย projects พร้อมกัน

**Skills Used:** trace, project, recap, fyi, where-we-are

**Workflow:**
```bash
# Start of day
/standup
/where-we-are

# Working on checkout flow
/trace "checkout"
/context-finder "checkout"

# Encountering bug
/feel frustrated trigger:bug-deadline
/trace "checkout error"
/project find "checkout"

# End of day
/recap
/forward
```

**Benefits:**
- Never lose context when switching projects
- Quick access to all relevant information
- Track emotions and identify patterns
- Seamless continuation across sessions

---

### Use Case 2: Student Learning

**Scenario:** คุณคือ student ที่เรียนรู้ topics ใหม่ๆ

**Skills Used:** learn, watch, trace, recap, where-we-are

**Workflow:**
```bash
# Start learning
/where-we-are

# Learn new topic
/watch https://www.youtube.com/watch?v=xyz
/trace "learned topics"

# Review
/recap
/feel excited

# Continue next day
/standup
```

**Benefits:**
- Structured learning process
- Never forget what you've learned
- Track progress naturally
- Stay motivated with emotion tracking

---

### Use Case 3: Project Manager

**Scenario:** Managing multiple projects และ stakeholder expectations

**Skills Used:** project, standup, recap, fyi, trace

**Workflow:**
```bash
# Morning standup
/standup

# Check project status
/project list
/project status "project-a"

# Share updates
/fyi "Project-A milestone reached"

# End of day
/rrr
/recap
```

**Benefits:**
- Clear project visibility
- Easy knowledge sharing
- Structured updates
- Learn from past projects

---

### Use Case 4: Healthcare Worker

**Scenario:** Recording patient interactions และ case notes

**Skills Used:** feel, fyi, trace, recap, where-we-are

**Workflow:**
```bash
# Start shift
/where-we-are

# Document patient interaction
/fyi "Patient #123: Complained of headache, prescribed acetaminophen"
/feel concerned trigger:patient-complaint

# End shift
/recap
/forward
```

**Benefits:**
- Accurate documentation
- Track case patterns
- Share knowledge with team
- Clear handoff between shifts

---

### Use Case 5: Creative Writer

**Scenario:** Writing stories, articles, หรือ content

**Skills Used:** trace, fyi, recap, where-we-are, rrr

**Workflow:**
```bash
# Start writing
/where-we-are

# Research topic
/trace "writing techniques"
/fyi "Show, don't tell is key"

# Write draft
# (Write content...)

# Review
/recap
/feel proud

# Weekly reflection
/rrr
```

**Benefits:**
- Easy research access
- Maintain writing flow
- Track progress
- Learn from past writing

---

## Real-World Applications

### Education Sector

**For Teachers:**
- Track student progress
- Document lesson plans
- Share resources
- Reflect on teaching effectiveness

**For Students:**
- Study efficiently
- Track learning progress
- Organize notes
- Prepare for exams

### Healthcare Sector

**For Medical Professionals:**
- Document patient interactions
- Track case patterns
- Share knowledge
- Ensure continuity of care

**For Patients:**
- Track health journey
- Document symptoms
- Share with care team
- Monitor improvements

### Government Sector

**For Government Employees:**
- Document policies
- Track projects
- Share information
- Ensure accountability

**For Citizens:**
- Access government information
- Track applications
- Participate in governance
- Get updates on issues

### Private Sector

**For Corporate Employees:**
- Track work projects
- Document knowledge
- Share information
- Improve productivity

**For Entrepreneurs:**
- Track business progress
- Document decisions
- Share with team
- Scale operations

---

## Best Practices

### 1. Start Simple

**Don't try to use all skills at once.**

```bash
# Start with 3 skills
/feel          # Track emotions
/where-we-are  # Check status
/recap         # Review progress
```

**Then add more as you get comfortable.**

### 2. Be Consistent

**Use skills daily, not just when you remember.**

- `/feel` every day (track emotions)
- `/standup` daily (daily progress)
- `/recap` weekly (weekly review)

### 3. Be Honest

**Be honest about your emotions and progress.**

```bash
/feel tired energy:2
# Don't say "good" if you're exhausted
```

### 4. Review Regularly

**Don't just add, review what you've added.**

```bash
# Weekly review
/rrr
# Analyze patterns and adjust your approach
```

### 5. Customize

**Make skills work for you, not the other way around.**

```json
{
  "skills": {
    "autoInvoke": ["your-favorite-skills"]
  }
}
```

### 6. Share with Team

**If you're working with others, share your knowledge.**

```bash
/fyi "Found a good resource for: topic"
/trace "team-work"  # Share what you're working on
```

### 7. Learn from Mistakes

**Use retrospectives to learn.**

```bash
/rrr
# What went wrong? Why? What can we do better?
```

### 8. Balance Work and Rest

**Track when you're burned out.**

```bash
/feel burned-out trigger:overwork
# Take a break
```

---

## Real-World Examples

### Example 1: Daily Developer Routine

```bash
# 8:00 AM - Start of day
/standup
/where-we-are

# 9:00 AM - Working on project
/project status "openclaw"
/trace "api integration"

# 11:00 AM - Encounter issue
/feel frustrated trigger:api-error
/trace "api integration error"

# 12:00 PM - Lunch break
/feel relaxed energy:5

# 1:00 PM - Continue working
/context-finder "api"
/fyi "API rate limit is 1000 req/hour"

# 5:00 PM - End of day
/recap
/forward "Tomorrow: finish API integration"

# 7:00 PM - Evening reflection
/feel tired energy:2
```

**Outcome:**
- Track emotions throughout the day
- Document technical decisions
- Don't lose context overnight
- Ready to continue next day

---

### Example 2: Student Learning Journey

```bash
# Monday 9:00 AM - Start learning
/standup
/where-we-are

# Monday 10:00 AM - Watch video
/watch https://www.youtube.com/watch?v=xyz
/feel confused trigger:complex-concept

# Monday 11:00 AM - Research
/trace "complex concepts"

# Monday 2:00 PM - Practice
/fyi "Practice makes perfect"
/feel confident energy:4

# Monday 5:00 PM - End day
/recap
/forward "Tomorrow: continue practice"

# Wednesday 9:00 AM - New week
/standup
/recap

# Friday 5:00 PM - End week
/rrr
# Analyze what worked and what didn't
```

**Outcome:**
- Track learning progress
- Document confusion points
- See improvement over time
- Learn from mistakes

---

### Example 3: Project Manager Weekly Review

```bash
# Monday 9:00 AM - Weekly standup
/standup

# Monday 10:00 AM - Check projects
/project list

# Monday 2:00 PM - Share updates
/fyi "Project A: Milestone reached"
/fyi "Project B: On track"

# Wednesday 5:00 PM - Mid-week check
/recap
/where-we-are

# Friday 5:00 PM - Weekly retrospective
/rrr

# Friday 6:00 PM - Plan next week
/recap
/forward "Next week: focus on Project A"

# Next Monday 9:00 AM - New week
/standup
```

**Outcome:**
- Clear weekly goals
- Easy progress tracking
- Structured team updates
- Learning from past weeks

---

## Troubleshooting

### Skill Not Loading

**Problem**: Skills don't appear when you list them

**Solutions**:
1. Check if skill folder exists:
   ```bash
   ls ~/.openclaw/workspace/skills/oracle-skills/
   ```

2. Check configuration:
   ```bash
   cat ~/.openclaw/openclaw.json | grep -A 10 "skills"
   ```

3. Restart OpenClaw:
   ```bash
   openclaw gateway restart
   ```

4. Reinstall skills:
   ```bash
   cd ~/.openclaw/workspace/skills
   git pull
   ```

### Heartbeat Not Working

**Problem**: Tasks in HEARTBEAT.md not being executed

**Solutions**:
1. Check heartbeat file exists:
   ```bash
   ls ~/.openclaw/workspace/HEARTBEAT.md
   ```

2. Verify heartbeat format:
   ```markdown
   # Format must match exactly:
   # - Check every X minutes:
   # - Commands in list format
   ```

3. Check OpenClaw is running:
   ```bash
   openclaw gateway status
   ```

### Emotion Tracking Not Working

**Problem**: `/feel` command doesn't log emotions

**Solutions**:
1. Check feelings folder exists:
   ```bash
   ls ~/.openclaw/workspace/memory/feelings/
   ```

2. Check log file permissions:
   ```bash
   chmod 644 ~/.openclaw/workspace/memory/feelings/2026-02-25.log
   ```

3. Check file format:
   ```bash
   tail ~/.openclaw/workspace/memory/feelings/2026-02-25.log
   ```

---

## FAQ

### Q1: Oracle Skills ฟรีไหม?

**A**: ใช่ครับ 100% ฟรีและ open-source!

### Q2: มันเก็บข้อมูลที่ server ไหม?

**A**: ไม่ครับ! ข้อมูลทุกอย่างเก็บ locally ที่เครื่องของคุณเอง

### Q3: ใช้งาน offline ได้ไหม?

**A**: ใช่ครับ! หลังจากติดตั้งเสร็จ ไม่ต้องออก internet (ยกเว้นตอนติดตั้งครั้งแรก)

### Q4: ทำงานได้กับทุก AI model ไหม?

**A**: ใช่ครับ ทำงานได้ดีที่สุดกับ OpenClaw native AI แต่สามารถปรับใช้กับอื่นๆ ได้

### Q5: สามารถปรับแต่ง skills ได้ไหม?

**A**: ได้ครับ! สามารถแก้ไข SKILL.md file หรือสร้าง custom skills โดยใช้ `/skill-creator`

### Q6: ปลอดภัยไหม?

**A**: ใช่ครับ! ข้อมูลเก็บ locally ที่เครื่องของคุณเอง ไม่มีการส่งข้อมูลไป server ภายนอก

### Q7: สามารถแชร์ข้อมูลกับคนอื่นไหม?

**A**: ได้ครับ! คุณควบคุมข้อมูลของคุณเอง แชร์สิ่งที่ต้องการ คงสิ่งที่ไม่ต้องการ

### Q8: ใช้ AI วิเคราะห์ข้อมูลไหม?

**A**: ใช่ครับ แต่เป็น local AI ที่ช่วยจัดระเบียบและเข้าใจข้อมูลของคุณ

### Q9: ใช้ได้บน mobile ไหม?

**A**: ใช่ครับ OpenClaw supports mobile devices

### Q10: ถ้าอยากถอนการติดตั้งไหม?

**A**: ง่ายครับ:
```bash
cd ~/.openclaw/workspace/skills
rm -rf oracle-skills
# Edit openclaw.json to remove skills configuration
```

### Q11: ทำงานร่วมกับ app อื่นได้ไหม?

**A**: สามารถ integrate ได้ครับ แต่ best เป็น standalone system

### Q12: ใช้งานร่วมกับทีมได้ไหม?

**A**: ได้ครับ! แชร์ memory/log files ให้ทีมคุณ

### Q13: ทำงานได้ทุก operating system ไหม?

**A**: ใช่ครับ ทำงานได้ทั้ง Linux, macOS, และ Windows

### Q14: ควรใช้ skills บ่อยแค่ไหน?

**A**: ควรใช้ `/feel` daily, `/standup` daily, และ `/recap` weekly ครับ

### Q15: ใช้ skills ได้พร้อมกันไหม?

**A**: ได้ครับ! คุณสามารถ invoke multiple skills ในคำสั่งเดียว

### Q16: เรียนรู้จาก usage ไหม?

**A**: ใช่ครับ เข้าใจ pattern และ preference ของคุณได้ตลอด

### Q17: สามารถ export data ไหม?

**A**: ได้ครับ ข้อมูลทั้งหมด stored ใน plain text files

### Q18: ทำงานกับ multiple languages ไหม?

**A**: ได้ครับ รองรับทุก language สำหรับ content

### Q19: ใช้สำหรับ personal และ work projects ได้ไหม?

**A**: ได้ครับ! ใช้ different folders or tags แยก

### Q20: ถ้าไม่เทคนิคควรเริ่มยังไง?

**A**: อ่าน guide นี้ แล้วเริ่มใช้ `/feel` และ `/where-we-are` ครับ

---

## Conclusion

### ทำไมต้อง Oracle Skills?

Oracle Skills ให้ combination ที่ unique:

1. **Automation** - No manual command typing needed
2. **Context Awareness** - Always know where you are
3. **Emotional Tracking** - Understand your mental state
4. **Proactive Assistance** - Help before being asked
5. **Privacy** - All data stays local
6. **Flexibility** - Customize to your needs
7. **Community** - Open-source and evolving

### คนไหนควรใช้ Oracle Skills?

**Perfect สำหรับ:**
- Developers working on multiple projects
- Students learning complex topics
- Project managers tracking multiple initiatives
- Healthcare workers documenting care
- Creative professionals tracking ideas
- Anyone who wants to stay organized

**Great สำหรับ:**
- Anyone wanting to improve productivity
- Anyone wanting to stay focused
- Anyone wanting to track their journey
- Anyone wanting to share knowledge

### Getting Started

1. **Install OpenClaw**
   ```bash
   npm install -g openclaw
   ```

2. **Install Oracle Skills**
   ```bash
   cd ~/.openclaw/workspace/skills
   git clone https://github.com/Soul-Brews-Studio/plugin-marketplace.git .
   ```

3. **Configure**
   - Edit openclaw.json
   - Create HEARTBEAT.md
   - Set up memory structure

4. **Start Using**
   ```bash
   /feel "Good morning!"
   /where-we-are
   /standup
   ```

### Final Thoughts

Oracle Skills is more than just a set of commands – it's a way to enhance your work, learn more efficiently, and stay organized without manual effort.

**"The Oracle Keeps the Human Human"** – Use AI to augment, not replace, your capabilities.

Start small, be consistent, and let Oracle Skills help you achieve more with less effort.

---

## Additional Resources

- **OpenClaw Documentation**: https://docs.openclaw.ai
- **GitHub Repository**: https://github.com/Soul-Brews-Studio/plugin-marketplace
- **Community Discord**: https://discord.com/invite/clawd
- **Getting Started Guide**: https://docs.openclaw.ai/guide

---

## Acknowledgments

- **Oracle Philosophy**: Inspired by the Oracle Skills philosophy
- **Soul Brews Studio**: Original creators of Oracle Skills
- **OpenClaw Community**: Feedback and improvements
- **Contributors**: All who contribute to open-source

---

**Version History**:
- 1.0 (2026-02-25): Initial comprehensive guide in Thai

**Last Updated**: 2026-02-25

**License**: MIT

**Stay Updated**: Join the OpenClaw community for updates and best practices!

---

*"The Oracle Keeps the Human Human"*

---

*End of Complete Guide (Thai)*
