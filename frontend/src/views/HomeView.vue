<template>
  <div class="home">

    <!-- ① HERO ─────────────────────────────────── -->
    <section class="hero">
      <div class="hero-text">
        <div class="hero-tag">For migrant women in Australia</div>

        <!--
          H1 split across two lines intentionally.
          The <em> adds the italic serif accent on "make sense"
          which softens the heading and adds personality.
        -->
        <h1>Making Australian healthcare<br><em>make sense</em> — for you.</h1>

        <p class="hero-sub">
          Confused about where to go? Worried about the cost?
          You're not alone — and you don't have to figure it out by yourself.
        </p>

        <div class="hero-actions">
          <!--
            Only ONE primary CTA here. The whole point of the hero is
            to funnel the user to one action: the triage guide.
            "Find a doctor" is secondary — ghost style, lower visual weight.
          -->
          <RouterLink to="/guide" class="btn-hero">Get guidance now →</RouterLink>
          <RouterLink to="/care" class="btn-ghost">Find a doctor</RouterLink>
        </div>
      </div>

      <div class="hero-visual">
        <div class="blob-bg" />

        <!-- Floating info cards — reinforce the two core fears -->
        <div class="card-float card1">
          <div class="card-icon"><i class="ti ti-map-pin-check" aria-hidden="true" /></div>
          <div class="card-label">Not sure where to go?</div>
          <div class="card-value primary">We'll tell you</div>
          <div class="checklist-row">
            <div class="check-dot"><i class="ti ti-check" /></div>
            GP, pharmacy, or emergency
          </div>
        </div>

        <div class="card-float card2">
          <div class="card-icon card-icon--accent"><i class="ti ti-receipt" aria-hidden="true" /></div>
          <div class="card-label">Worried about cost?</div>
          <div class="card-value">Know before you go</div>
          <div class="checklist-row">
            <div class="check-dot"><i class="ti ti-check" /></div>
            Bulk billing explained
          </div>
        </div>

        <div class="card-float card3">
          <div class="card-label">Trusted sources</div>
          <div class="card-value primary" style="font-size:12px; line-height:1.4;">
            Australian Government<br>health authorities
          </div>
        </div>
      </div>
    </section>

    <!-- ② TRUST ──────────────────────────────────
         Why should she believe WithHer?
         Three clear, honest reasons.
    ─────────────────────────────────────────────── -->
    <section class="trust">
      <div class="trust-inner">
        <div
          v-for="item in trustItems"
          :key="item.label"
          class="trust-item"
        >
          <div class="trust-icon">
            <i :class="'ti ti-' + item.icon" aria-hidden="true" />
          </div>
          <div class="trust-body">
            <div class="trust-headline">{{ item.headline }}</div>
            <div class="trust-label">{{ item.label }}</div>
          </div>
        </div>
      </div>
    </section>

    <!-- ③ FEATURES ───────────────────────────────
         What can she do here?
         Copy rewritten to speak to her situation, not describe the product.
    ─────────────────────────────────────────────── -->
    <section class="features">
      <div class="section-eyebrow">What WithHer does</div>
      <div class="section-title">Everything in one place</div>
      <p class="section-sub">
        From "where do I even go?" to finding a bulk-billing clinic near you —
        we've got it covered.
      </p>

      <div class="feature-grid">
        <!--
          Each feature card is a RouterLink — the whole card is clickable.
          :class="feature.variant" adds the highlight/accent modifier if set.
        -->
        <RouterLink
          v-for="feature in features"
          :key="feature.name"
          :to="feature.route"
          class="feature-card"
          :class="feature.variant"
        >
          <div class="feature-icon" :class="'icon-' + feature.iconColor">
            <i :class="'ti ti-' + feature.icon" aria-hidden="true" />
          </div>
          <div class="feature-name">{{ feature.name }}</div>
          <p class="feature-desc">{{ feature.desc }}</p>
          <span class="feature-tag" :class="feature.tagClass">{{ feature.tag }}</span>
        </RouterLink>
      </div>
    </section>

    <!-- ④ CTA ────────────────────────────────────
         Repeat the invitation. Some users scroll all the way down
         before they're ready to act — meet them here too.
    ─────────────────────────────────────────────── -->
    <section class="features" style="padding-top: 0;">
      <div class="cta-section">
        <h2>Not sure where to start?</h2>
        <p>
          Tell us what's going on — we'll walk you through it.
          No jargon, no judgment, just clear next steps.
        </p>
        <RouterLink to="/guide" class="cta-btn">Start here →</RouterLink>
      </div>
    </section>

  </div>
</template>

<script setup>
/*
  Data lives here, not hardcoded in the template.
  This makes it easy to update copy without touching the HTML structure.
  When we connect a real backend, these arrays get replaced with API calls.
*/

const trustItems = [
  {
    icon: 'shield-check',
    headline: 'Government-backed information',
    label: 'Health content sourced from Australian Government health authorities, including healthdirect.gov.au',
  },
  {
    icon: 'language',
    headline: 'Available in 6+ languages',
    label: 'Find a GP or clinic that speaks your language — no awkward translations needed',
  },
  {
    icon: 'coin',
    headline: 'Understand what you\'ll pay',
    label: 'We explain bulk billing, Medicare, and out-of-pocket costs in plain language — before you book',
  },
]

const features = [
  {
    name: 'Not sure where to go?',
    /*
      Feature copy rewritten to speak to her situation.
      Old: "A step-by-step onboarding guide that explains Medicare..."
      New: Starts with her question, ends with what she gets.
    */
    desc: 'GP, emergency room, or pharmacy? Answer a few questions and we\'ll tell you exactly where to go and what to expect.',
    icon: 'map-pin',
    iconColor: 'navy',
    variant: 'highlight',
    tag: 'Start here',
    tagClass: 'tag-white',
    route: '/guide',
  },
  {
    name: 'Find a bulk-billing clinic',
    desc: 'Search clinics near you filtered by bulk billing, language spoken, and telehealth. Know the costs before you book.',
    icon: 'stethoscope',
    iconColor: 'blush',
    variant: '',
    tag: 'Find care',
    tagClass: '',
    route: '/care',
  },
  {
    name: 'Understand the system',
    desc: 'What does Medicare actually cover? Do I need private insurance? Get clear answers — written like a knowledgeable friend, not a government website.',
    icon: 'book-2',
    iconColor: 'soft',
    variant: '',
    tag: 'Read guides',
    tagClass: '',
    route: '/learn',
  },
  {
    name: 'Your health checklist',
    desc: 'Which screenings should you be booking at your age? We\'ll tell you what to prioritise and how to get them covered.',
    icon: 'checklist',
    iconColor: 'blush',
    variant: '',
    tag: 'For you',
    tagClass: '',
    route: '/hub',
  },
  {
    name: 'My health hub',
    desc: 'Save clinics you trust, track your checklist, and keep notes about your health — all in one private place.',
    icon: 'user-heart',
    iconColor: 'blush',
    variant: '',
    tag: 'Sign in to access',
    tagClass: 'tag-muted',
    route: '/hub',
  },
  {
    name: 'Community & support',
    desc: 'You\'re not the only one navigating this. Connect with other migrant women, ask questions, and share what you know.',
    icon: 'users',
    iconColor: 'soft',
    variant: '',
    tag: 'Join the community',
    tagClass: '',
    route: '/community',
  },
]
</script>

<style scoped>
.home {
  --primary:       #1B3A5C;
  --primary-light: #e8eef5;
  --primary-mid:   #2d5480;
  --accent:        #E8B4B8;
  --accent-light:  #fdf5f5;
  --accent-dark:   #b8737a;
  --surface:       #FFFFFF;
  --text:          #0D1F33;
  --text-mid:      #4a5a6b;
  --text-light:    #8a9bb0;
  --border:        #e2e8f0;
  --radius:        16px;

  font-family: 'DM Sans', sans-serif;
  color: var(--text);
}

/* ── HERO ─────────────────────────────────────── */
.hero {
  display: grid;
  grid-template-columns: 1fr 1fr;
  max-width: 1100px;
  margin: 0 auto;
  padding: 72px 32px 80px;
  align-items: center;
  gap: 48px;
}

.hero-tag {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  background: var(--primary-light);
  color: var(--primary);
  font-size: 13px;
  font-weight: 500;
  padding: 5px 14px;
  border-radius: 100px;
  margin-bottom: 24px;
}

.hero-tag::before {
  content: '';
  width: 6px; height: 6px;
  border-radius: 50%;
  background: var(--accent);
}

h1 {
  font-family: 'Lora', serif;
  font-size: 58px;
  font-weight: 600;
  line-height: 1.14;
  color: var(--text);
  margin-bottom: 24px;
  letter-spacing: -1px;
}

h1 em { font-style: italic; color: var(--primary); }

.hero-sub {
  font-size: 18px;
  color: var(--text-mid);
  line-height: 1.7;
  margin-bottom: 36px;
  font-weight: 300;
  max-width: 420px;
}

.hero-actions { display: flex; gap: 12px; align-items: center; flex-wrap: wrap; }

.btn-hero {
  background: var(--primary);
  color: white;
  padding: 14px 28px;
  border-radius: 100px;
  font-size: 15px;
  font-weight: 500;
  text-decoration: none;
  display: inline-block;
  transition: background 0.2s, transform 0.15s;
}
.btn-hero:hover { background: var(--primary-mid); transform: translateY(-1px); }

.btn-ghost {
  background: transparent;
  color: var(--text-mid);
  border: 0.5px solid var(--border);
  padding: 14px 24px;
  border-radius: 100px;
  font-size: 15px;
  text-decoration: none;
  display: inline-block;
  transition: all 0.2s;
}
.btn-ghost:hover { border-color: var(--primary); color: var(--primary); }

/* ── HERO VISUAL ──────────────────────────────── */
.hero-visual {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 420px;
}

.blob-bg {
  position: absolute;
  width: 360px; height: 360px;
  background: var(--accent-light);
  border-radius: 60% 40% 70% 30% / 50% 60% 40% 50%;
  animation: morph 8s ease-in-out infinite;
}

@keyframes morph {
  0%, 100% { border-radius: 60% 40% 70% 30% / 50% 60% 40% 50%; }
  50%       { border-radius: 40% 60% 30% 70% / 60% 40% 55% 45%; }
}

.card-float {
  position: absolute;
  background: var(--surface);
  border-radius: var(--radius);
  padding: 14px 18px;
  box-shadow: 0 2px 20px rgba(13, 31, 51, 0.09);
  font-size: 13px;
}

.card1 { top: 24px; right: 0; width: 192px; }
.card2 { bottom: 48px; left: 0; width: 196px; }
.card3 { top: 50%; right: -16px; transform: translateY(-50%); width: 164px; }

.card-icon          { font-size: 20px; margin-bottom: 6px; color: var(--primary); }
.card-icon--accent  { color: var(--accent-dark); }

.card-label  { font-size: 11px; color: var(--text-light); text-transform: uppercase; letter-spacing: 0.5px; margin-bottom: 4px; }
.card-value  { font-weight: 500; font-size: 14px; color: var(--text); }
.card-value.primary { color: var(--primary); }

.checklist-row { display: flex; align-items: center; gap: 6px; margin-top: 8px; font-size: 12px; color: var(--text-mid); }
.check-dot {
  width: 16px; height: 16px; flex-shrink: 0;
  border-radius: 50%;
  background: var(--primary-light);
  display: flex; align-items: center; justify-content: center;
  font-size: 9px; color: var(--primary);
}

/* ── TRUST ────────────────────────────────────── */
/*
  Full-width band with soft border top and bottom.
  Sits between Hero and Features as a visual pause — "why trust us?"
*/
.trust {
  border-top: 0.5px solid var(--border);
  border-bottom: 0.5px solid var(--border);
  background: var(--surface);
  padding: 40px 32px;
}

.trust-inner {
  max-width: 1100px;
  margin: 0 auto;
  display: grid;
  /*
    Three equal columns. auto-fit with minmax would work too,
    but we know we always have exactly 3 items here.
  */
  grid-template-columns: repeat(3, 1fr);
  gap: 32px;
}

.trust-item {
  display: flex;
  gap: 16px;
  align-items: flex-start;
}

.trust-icon {
  width: 40px; height: 40px; flex-shrink: 0;
  border-radius: 10px;
  background: var(--primary-light);
  color: var(--primary);
  display: flex; align-items: center; justify-content: center;
  font-size: 18px;
}

.trust-headline {
  font-size: 14px;
  font-weight: 600;
  color: var(--text);
  margin-bottom: 4px;
}

.trust-label {
  font-size: 13px;
  color: var(--text-mid);
  font-weight: 300;
  line-height: 1.6;
}

/* ── FEATURES ─────────────────────────────────── */
.features {
  max-width: 1100px;
  margin: 0 auto;
  padding: 80px 32px 64px;
}

.section-eyebrow { font-size: 13px; text-transform: uppercase; letter-spacing: 1.5px; color: var(--text-light); margin-bottom: 10px; }
.section-title   { font-family: 'Lora', serif; font-size: 38px; font-weight: 600; color: var(--text); margin-bottom: 12px; }
.section-sub     { color: var(--text-mid); font-size: 17px; font-weight: 300; line-height: 1.7; margin-bottom: 40px; max-width: 520px; }

.feature-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 16px;
}

.feature-card {
  background: var(--surface);
  border-radius: var(--radius);
  padding: 24px;
  border: 0.5px solid var(--border);
  text-decoration: none;
  color: inherit;
  display: block;
  transition: transform 0.2s, box-shadow 0.2s;
}
.feature-card:hover     { transform: translateY(-3px); box-shadow: 0 8px 28px rgba(13,31,51,0.08); }
.feature-card.highlight { background: var(--primary); border-color: var(--primary); color: white; }

.feature-icon {
  width: 40px; height: 40px;
  border-radius: 10px;
  display: flex; align-items: center; justify-content: center;
  margin-bottom: 16px;
  font-size: 18px;
}
.icon-navy  { background: var(--primary-light); color: var(--primary); }
.icon-blush { background: var(--accent-light); color: var(--accent-dark); }
.icon-soft  { background: #eef2f7; color: var(--primary-mid); }

.feature-name { font-weight: 600; font-size: 16px; margin-bottom: 8px; }
.feature-desc { font-size: 14px; color: var(--text-mid); line-height: 1.65; font-weight: 300; }
.highlight .feature-desc { color: rgba(255,255,255,0.75); }

.feature-tag {
  display: inline-block;
  margin-top: 16px;
  font-size: 11px; font-weight: 500;
  color: var(--primary);
  background: var(--primary-light);
  padding: 3px 10px;
  border-radius: 100px;
}
.tag-white { background: rgba(255,255,255,0.18); color: white; }
.tag-muted { background: #eef0f3; color: var(--text-light); }

/* ── CTA ──────────────────────────────────────── */
.cta-section {
  background: var(--primary);
  border-radius: var(--radius);
  padding: 56px 48px;
  text-align: center;
  position: relative;
  overflow: hidden;
}

.cta-section::before {
  content: ''; position: absolute;
  top: -80px; right: -80px;
  width: 300px; height: 300px;
  background: rgba(255,255,255,0.04); border-radius: 50%;
}
.cta-section::after {
  content: ''; position: absolute;
  bottom: -60px; left: -40px;
  width: 220px; height: 220px;
  background: rgba(255,255,255,0.04); border-radius: 50%;
}

.cta-section h2 {
  font-family: 'Lora', serif;
  font-size: 42px; font-weight: 600;
  color: white;
  margin-bottom: 16px;
  position: relative; z-index: 1;
}
.cta-section p {
  color: rgba(255,255,255,0.72);
  font-size: 17px; font-weight: 300; line-height: 1.7;
  margin-bottom: 32px;
  position: relative; z-index: 1;
  max-width: 440px; margin-left: auto; margin-right: auto;
}

.cta-btn {
  background: var(--surface);
  color: var(--primary);
  padding: 14px 36px;
  border-radius: 100px;
  font-size: 15px; font-weight: 500;
  text-decoration: none;
  display: inline-block;
  position: relative; z-index: 1;
  transition: all 0.2s;
}
.cta-btn:hover { transform: translateY(-1px); box-shadow: 0 4px 20px rgba(0,0,0,0.15); }

/* ── RESPONSIVE ───────────────────────────────── */
@media (max-width: 768px) {
  .hero {
    grid-template-columns: 1fr;
    padding: 40px 20px 48px;
    gap: 48px;
  }
  h1 { font-size: 42px; letter-spacing: -0.5px; }
  .hero-visual { height: 320px; }
  .blob-bg { width: 280px; height: 280px; }
  .card1 { top: 10px; right: 0; width: 170px; }
  .card2 { bottom: 20px; left: 0; width: 176px; }
  .card3 { right: -8px; width: 150px; }

  .trust { padding: 32px 20px; }
  .trust-inner { grid-template-columns: 1fr; gap: 24px; }

  .features { padding: 48px 20px 40px; }
  .feature-grid { grid-template-columns: 1fr; }

  .cta-section { padding: 40px 24px; }
  .cta-section h2 { font-size: 32px; }
}
</style>