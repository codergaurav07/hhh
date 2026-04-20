You are an expert frontend developer.
I have already created a prototype website, but it has multiple issues. I do NOT want a completely new project. I want you to MODIFY and FIX my existing code while keeping all features intact.
Project Name:
"EcoTrack – Smart Plastic Waste Management System"
-----------------------------------
❌ CURRENT PROBLEMS (FIX ALL)
-----------------------------------
1. Sections are overlapping instead of switching
2. Previous sections (like Dashboard) remain visible in background
3. Navigation is not working properly (user has to scroll)
4. Website breaks on mobile devices (not responsive)
5. Camera opens in a small area instead of full screen
6. UI feels slightly broken and unstructured
-----------------------------------
🎯 YOUR TASK
-----------------------------------
- Fix all bugs in the EXISTING code
- Do NOT remove features
- Improve structure and responsiveness
- Keep the same concept and sections
-----------------------------------
🧱 SECTION FIX (VERY IMPORTANT)
-----------------------------------
- Every section must have:
  class="section"
- Use this logic:
   → Hide ALL sections (display: none)
   → Show ONLY selected section (display: block)
- Ensure:
   → No section remains visible in background
   → No overlapping
-----------------------------------
⚙️ JAVASCRIPT FIX
-----------------------------------
Create or fix function:
function showSection(id) {
  // hide all sections
  // remove active class
  // show selected section
  // add active class
}
- Set default section = Home on page load
-----------------------------------
📱 MOBILE + RESPONSIVENESS FIX
-----------------------------------
- Add viewport meta tag
- Make layout responsive for Android/mobile
- Fix overlapping UI
- Use flexible layout (flexbox/grid)
- Ensure each section fits full screen
-----------------------------------
📷 CAMERA FIX (VERY IMPORTANT)
-----------------------------------
- When user clicks "Open Camera":
   → Ask permission
   → Open camera in FULL SCREEN MODAL (like Google Pay scanner)
   → Dark background overlay
   → Video should cover full screen
   → Add "Close Camera" button
- When closing:
   → Stop camera stream
   → Hide modal properly
-----------------------------------
🔐 LOGIN SYSTEM
-----------------------------------
- Keep login/signup screen
- Add simple validation
- After login → show main app
-----------------------------------
🎨 UI IMPROVEMENT
-----------------------------------
- Fix alignment issues
- Improve spacing
- Keep green eco theme
- Add smooth transitions
- Highlight active navbar button
-----------------------------------
📄 KEEP ALL SECTIONS SAME
-----------------------------------
Do NOT remove:
- Home
- Dashboard
- Scan
- Rewards
- Map
- Smart Dustbin
- Leaderboard
- Profile
- Contact
-----------------------------------
💡 EXTRA (IMPORTANT)
-----------------------------------
- Keep "Plastic Credit Economy" concept
- Add simple eco messages
- Keep UI realistic (not overly perfect)
-----------------------------------
🎯 FINAL OUTPUT
-----------------------------------
- Return ONE complete HTML file
- Include updated HTML + CSS + JS
- Fully working version
- No overlapping sections
- Navigation works perfectly
- Mobile + desktop both working
- Camera full screen working
- Clean and well-commented code
Modify the existing code properly. Do not create a completely different project.
