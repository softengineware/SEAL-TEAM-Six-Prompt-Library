# ST6 Memory System Diagnostic & Recovery Protocol

## 🚨 CRITICAL ALERT: Memory Systems Offline

### Current Situation
- **Mission Progress**: 22% complete (11/50 prompts transformed)
- **Memory Backup**: NOT CONFIRMED
- **Risk Level**: HIGH - Session data may be lost on restart

### Memory System Status

#### 1. mem0 MCP Server
**Status**: OFFLINE ❌
**Expected Tools**: 
- mcp__mem0__add_memory
- mcp__mem0__get_memories
- mcp__mem0__search_memories

**Startup Protocol**:
```bash
# Check if mem0 is installed
ls ~/Library/Application\ Support/Claude/claude_desktop_config.json

# Start mem0 server (from separate terminal)
cd ~/path/to/mem0/mcp
npm start

# Or if using Python version
python -m mem0.mcp.server
```

#### 2. Supabase MCP Server
**Status**: OFFLINE ❌
**Expected Project**: KNOWLEDGE
**Expected Tools**:
- mcp__supabase__insert
- mcp__supabase__query
- mcp__supabase__update

**Startup Protocol**:
```bash
# Check Supabase configuration
cat ~/Library/Application\ Support/Claude/claude_desktop_config.json | grep -A 10 supabase

# Start Supabase MCP (from separate terminal)
cd ~/path/to/supabase/mcp
npm start
```

### Troubleshooting Steps

1. **Check Claude Desktop Configuration**:
```bash
cat ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

2. **Verify MCP Server Installations**:
```bash
# Check for MCP installations
find ~ -name "*mcp*" -type d 2>/dev/null | grep -E "(mem0|supabase)"
```

3. **Manual Memory Backup** (Current Session):
```bash
# Create emergency backup
mkdir -p ~/ST6_Backup/$(date +%Y%m%d_%H%M%S)
cp -r /Users/branchechols/LLM-Prompt-Library ~/ST6_Backup/$(date +%Y%m%d_%H%M%S)/
```

### Recovery Protocol

If memory systems cannot be started:

1. **Save Current State**:
   - All work is committed to git ✅
   - MISSION_CONTINUITY.md contains full status ✅
   - TASKS.md tracks all objectives ✅

2. **Restart Claude Code**:
   ```
   1. Close Claude Code
   2. Start mem0 MCP server in terminal
   3. Start Supabase MCP server in terminal
   4. Reopen Claude Code
   5. Verify MCP tools are available
   ```

3. **Resume Mission**:
   - Open MISSION_CONTINUITY.md
   - Continue from "prompts/programming/priompt_style_cursor_IDE"
   - Follow TASKS.md for remaining objectives

### Alternative Memory Solutions

If MCP servers remain offline:

1. **Local File Storage**:
   - Continue saving progress in markdown files
   - Use git as primary backup mechanism
   - Create session logs manually

2. **External Backup**:
   ```bash
   # Push to backup branch
   git checkout -b memory-backup-$(date +%Y%m%d)
   git add -A
   git commit -m "ST6: Emergency memory backup"
   git push origin memory-backup-$(date +%Y%m%d)
   ```

### Mission Impact Assessment

**Without Memory Systems**:
- ❌ No automatic session persistence
- ❌ No cross-session context
- ❌ No searchable memory database
- ✅ Git tracking remains functional
- ✅ Local file system operational
- ✅ Mission can continue with manual tracking

### Recommended Action

1. **IMMEDIATE**: Try starting MCP servers manually
2. **IF FAILED**: Continue mission with git-based tracking
3. **DOCUMENT**: All decisions in markdown files
4. **COMMIT**: Frequently to preserve progress

---

**Mission Priority**: Continue ST6 transformations while documenting thoroughly.
**Backup Strategy**: Git commits every 2-3 transformations.
**Risk Mitigation**: Manual progress tracking in TASKS.md.