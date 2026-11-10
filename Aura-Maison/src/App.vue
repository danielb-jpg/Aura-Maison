<script setup>
import { ref, computed, onMounted } from 'vue'
import { WHATSAPP, categories, gallery, hero } from './data.js'

const active = ref(0)
const current = computed(() => categories[active.value])

const allServices = categories.flatMap((c) => c[2].map((x) => `${x[0]} (${x[1]})`))
const today = new Date().toISOString().split('T')[0]
const year = new Date().getFullYear()
const waLink = `https://wa.me/${WHATSAPP}?text=${encodeURIComponent('Hi Aura Maison, I’d like to book an appointment.')}`

const form = ref({ name: '', phone: '', service: allServices[0], date: '', time: '', notes: '' })

function book() {
  const f = form.value
  const lines = [
    'New booking request',
    `Name: ${f.name}`,
    `Phone: ${f.phone}`,
    `Service: ${f.service}`,
    `Date: ${f.date}`,
    `Time: ${f.time}`,
  ]
  if (f.notes) lines.push(`Notes: ${f.notes}`)
  window.open(`https://wa.me/${WHATSAPP}?text=${encodeURIComponent(lines.join('\n'))}`, '_blank')
}

// fade-in on scroll
onMounted(() => {
  const io = new IntersectionObserver(
    (entries) =>
      entries.forEach((e) => {
        if (e.isIntersecting) {
          e.target.classList.add('in')
          io.unobserve(e.target)
        }
      }),
    { threshold: 0.1 },
  )
  document.querySelectorAll('.rv').forEach((el) => io.observe(el))
})
</script>

<template>
  <svg width="0" height="0" style="position:absolute" aria-hidden="true">
    <symbol id="f" viewBox="0 0 200 260">
      <circle cx="100" cy="110" r="42" fill="var(--sk,#C98A8F)" />
      <path d="M52 135C42 62 75 36 100 36s58 26 48 99c-8-34-22-52-48-52s-40 18-48 52z" fill="var(--hr,#2A1A22)" />
      <path d="M10 260c0-70 40-100 90-100s90 30 90 100z" fill="var(--bd,#8E5A68)" />
    </symbol>
  </svg>

  <header class="nav">
    <div class="wrap">
      <a href="#top" class="brand">Aura <i>Maison</i></a>
      <ul>
        <li><a href="#services">Services</a></li>
        <li><a href="#wigs">Wig studio</a></li>
        <li><a href="#packages">Packages</a></li>
        <li><a href="#gallery">Gallery</a></li>
        <li><a href="#visit">Visit</a></li>
      </ul>
      <a href="#book" class="btn">Book now</a>
    </div>
  </header>

  <main id="top">
    <section class="hero dark">
      <div class="wrap hg">
        <div>
          <p class="eyebrow">Women’s hair &amp; beauty studio</p>
          <h1>Hair. Nails. Lashes. <em>All of it, done beautifully.</em></h1>
          <p class="s">From a flawless frontal install to a fresh mani-pedi, everything you need to walk out feeling expensive, under one roof.</p>
          <div class="cta">
            <a href="#book" class="btn">Book an appointment</a>
            <a href="#services" class="btn ghost">See the menu</a>
          </div>
        </div>
        <div class="tri">
          <div v-for="(h, i) in hero" :key="i" class="ar" :style="{ background: h.bg, '--hr': h.hr, '--bd': h.bd, '--sk': h.sk }">
            <svg viewBox="0 0 200 260" preserveAspectRatio="xMidYMax slice"><use href="#f" /></svg>
          </div>
        </div>
      </div>
    </section>

    <div class="strip">
      <span>Wig installs</span><span>Frontals</span><span>Knotless braids</span><span>Silk press</span>
      <span>Gel &amp; acrylics</span><span>Lashes</span><span>Gele</span><span>Bridal</span>
    </div>

    <section id="services">
      <div class="wrap">
        <p class="eyebrow rv">The menu</p>
        <h2 class="rv">Everything she <em>needs</em>, in one place</h2>
        <p class="lede rv">Starting prices based on typical Lagos salon rates. Final price depends on length, size and style.</p>
        <div class="menu">
          <nav class="cats" role="tablist">
            <button
              v-for="(c, i) in categories"
              :key="c[0]"
              class="cat"
              role="tab"
              :aria-selected="active === i"
              @click="active = i"
            >
              <small>{{ String(i + 1).padStart(2, '0') }}</small>{{ c[0] }}
            </button>
          </nav>
          <div class="list">
            <h3>{{ current[0] }}</h3>
            <p>{{ current[1] }}</p>
            <div v-for="x in current[2]" :key="x[0]" class="it">
              <b>{{ x[0] }}</b>
              <span v-if="x[2]">{{ x[2] }}</span>
              <i></i>
              <div class="p">{{ x[1] }}</div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section id="wigs" class="dark">
      <div class="wrap">
        <p class="eyebrow rv">Signature</p>
        <h2 class="rv">The <em>wig studio</em></h2>
        <p class="lede rv">Bring your wig or buy one here. We’ll make it sit like it grew from your own scalp.</p>
        <div class="steps">
          <div class="st rv"><div class="n">1</div><h3>Consult</h3><p>We talk length, density, lace type and the look you want.</p></div>
          <div class="st rv"><div class="n">2</div><h3>Customise</h3><p>Plucking, bleaching knots, cutting the lace and shaping the hairline.</p></div>
          <div class="st rv"><div class="n">3</div><h3>Install</h3><p>Frontal or closure, glued or glueless, laid flat and secure.</p></div>
          <div class="st rv"><div class="n">4</div><h3>Style</h3><p>Bone straight, curls, layers or a sleek bob. Finished to slay.</p></div>
        </div>
        <div class="tags rv">
          <span>Frontal install</span><span>Closure install</span><span>360 &amp; U-part</span><span>Glueless</span>
          <span>HD lace</span><span>Wig making</span><span>Revamp &amp; laundry</span><span>Frontal ponytail</span>
        </div>
      </div>
    </section>

    <section id="packages">
      <div class="wrap">
        <p class="eyebrow rv">Bundles</p>
        <h2 class="rv">Pamper <em>packages</em></h2>
        <div class="pk">
          <div class="pc rv">
            <h3>Girls’ Day</h3>
            <ul><li>Gel mani &amp; pedi</li><li>Wash &amp; blow-dry</li><li>Brow lamination</li></ul>
            <div class="p">From ₦70,000</div>
          </div>
          <div class="pc f rv">
            <h3>Bridal Glow</h3>
            <ul><li>Bridal trial + makeup</li><li>Gele tying</li><li>Wig or hair install</li><li>Mani, pedi &amp; lashes</li></ul>
            <div class="p">From ₦300,000</div>
          </div>
          <div class="pc rv">
            <h3>Birthday Slay</h3>
            <ul><li>Frontal install &amp; style</li><li>Full glam makeup</li><li>Acrylic set &amp; lash set</li></ul>
            <div class="p">From ₦130,000</div>
          </div>
        </div>
      </div>
    </section>

    <section id="gallery" style="background: var(--bg2)">
      <div class="wrap">
        <p class="eyebrow rv">The work</p>
        <h2 class="rv">Fresh from <em>the chair</em></h2>
        <div class="gal">
          <div
            v-for="g in gallery"
            :key="g[0]"
            class="tile rv"
            :style="{ aspectRatio: g[1], background: g[2], '--hr': g[3] }"
          >
            <svg viewBox="0 0 200 260" preserveAspectRatio="xMidYMax slice"><use href="#f" /></svg>
            <span>{{ g[0] }}</span>
          </div>
        </div>
      </div>
    </section>

    <section id="book" class="dark">
      <div class="wrap bk">
        <div class="rv">
          <p class="eyebrow">Reserve</p>
          <h2>Book your <em>glow-up</em></h2>
          <p class="lede">Pick a service and a time. Your request goes straight to us on WhatsApp and we’ll confirm shortly.</p>
        </div>
        <form class="rv" @submit.prevent="book">
          <div class="full"><label for="n">Your name</label><input id="n" v-model="form.name" required placeholder="Full name" /></div>
          <div class="full"><label for="p">Phone</label><input id="p" v-model="form.phone" type="tel" required placeholder="080…" /></div>
          <div class="full">
            <label for="s">Service</label>
            <select id="s" v-model="form.service" required>
              <option v-for="s in allServices" :key="s">{{ s }}</option>
            </select>
          </div>
          <div><label for="d">Date</label><input id="d" v-model="form.date" type="date" :min="today" required /></div>
          <div><label for="t">Time</label><input id="t" v-model="form.time" type="time" required /></div>
          <div class="full"><label for="x">Notes (optional)</label><input id="x" v-model="form.notes" placeholder="e.g. bringing my own wig" /></div>
          <div class="full"><button class="btn" type="submit">Send booking on WhatsApp</button></div>
        </form>
      </div>
    </section>

    <section id="visit">
      <div class="wrap">
        <p class="eyebrow rv">Find us</p>
        <h2 class="rv">Visit the <em>studio</em></h2>
        <div style="max-width: 620px; margin-top: 34px" class="rv">
          <div class="row"><span>Address</span><b>18 Admiralty Way, Lagos</b></div>
          <div class="row"><span>Mon – Sat</span><b>8am – 8pm</b></div>
          <div class="row"><span>Sunday</span><b>12pm – 6pm</b></div>
          <div class="row"><span>Phone</span><b>+234 800 000 0000</b></div>
        </div>
      </div>
    </section>
  </main>

  <footer>
    <div class="wrap">
      <span>© {{ year }} Aura Maison. All rights reserved.</span>
      <span>Appointments preferred, walk-ins welcome.</span>
    </div>
  </footer>

  <a class="cc" :href="waLink" target="_blank" rel="noopener" aria-label="Chat with the concierge on WhatsApp">
    <i></i><span>Concierge</span>
  </a>
</template>
