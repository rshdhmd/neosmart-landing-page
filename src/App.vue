<script setup>
import { computed, onMounted, onUnmounted, ref } from 'vue'

const EMAIL_TO = 'info@neosmartgroup.com'
const PHONE_E164 = '971529708692'
const PHONE_DISPLAY = '+971 52 970 8692'
const PROFILE_URL = 'https://neosmartgroup.com/about-us/company-overview/'
const GOOGLE_URL =
  'https://www.google.com/maps/search/?api=1&query=Neo+Smart+Laboratory+Equipment+Trading+LLC+Dubai'

const menuOpen = ref(false)
const stuck = ref(false)
const tried = ref(false)
const error = ref('')

const form = ref({
  name: '',
  company: '',
  email: '',
  phone: '',
  requirement: '',
})

const nameInvalid = computed(() => tried.value && !form.value.name.trim())
const requirementInvalid = computed(() => tried.value && !form.value.requirement.trim())
const emailInvalid = computed(() => {
  const email = form.value.email.trim()
  return tried.value && email && !isEmail(email)
})
const phoneInvalid = computed(() => {
  const phone = form.value.phone.trim()
  return tried.value && phone && !isPhone(phone)
})

function isEmail(value) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)
}

function isPhone(value) {
  const digits = value.replace(/\D/g, '')
  return digits.length >= 7 && digits.length <= 15
}

function enquiryText() {
  return [
    `Full name: ${form.value.name.trim()}`,
    `Company: ${form.value.company.trim() || '—'}`,
    `Email: ${form.value.email.trim() || '—'}`,
    `WhatsApp / mobile: ${form.value.phone.trim() || '—'}`,
    '',
    'Requirement:',
    form.value.requirement.trim(),
  ].join('\n')
}

function validate() {
  tried.value = true
  const name = form.value.name.trim()
  const email = form.value.email.trim()
  const phone = form.value.phone.trim()
  const requirement = form.value.requirement.trim()

  if (!name) {
    error.value = 'Please enter your full name.'
    return false
  }
  if (!requirement) {
    error.value = 'Please describe your requirement.'
    return false
  }
  if (!email && !phone) {
    error.value = 'Please provide an email or mobile number.'
    return false
  }
  if (email && !isEmail(email)) {
    error.value = 'Please enter a valid email address.'
    return false
  }
  if (phone && !isPhone(phone)) {
    error.value = 'Please enter a valid mobile number.'
    return false
  }

  error.value = ''
  return true
}

function prepareEmail() {
  if (!validate()) return
  const subject = encodeURIComponent(`Enquiry from ${form.value.name.trim()} — Neo Smart`)
  const body = encodeURIComponent(enquiryText())
  window.location.href = `mailto:${EMAIL_TO}?subject=${subject}&body=${body}`
}

function chatWhatsApp() {
  const started = [form.value.name, form.value.company, form.value.email, form.value.phone, form.value.requirement].some(
    (value) => value.trim(),
  )
  if (started && !validate()) return

  const text = started
    ? `Hello Neo Smart,\n\n${enquiryText()}`
    : 'Hello Neo Smart, I would like to discuss a requirement.'
  window.open(`https://wa.me/${PHONE_E164}?text=${encodeURIComponent(text)}`, '_blank', 'noopener')
}

function closeMenu() {
  menuOpen.value = false
}

function onScroll() {
  stuck.value = window.scrollY > 4
}

onMounted(() => {
  onScroll()
  window.addEventListener('scroll', onScroll, { passive: true })
})

onUnmounted(() => {
  window.removeEventListener('scroll', onScroll)
})
</script>

<template>
  <header class="site-header" :class="{ 'is-stuck': stuck }">
    <div class="wrap header-inner">
      <a class="logo" href="#top" @click="closeMenu">
        <img src="/logo-light.png" alt="Neo Smart — Accelerating Scientific Progress" />
      </a>

      <button
        class="menu-btn"
        type="button"
        aria-label="Menu"
        :aria-expanded="menuOpen"
        aria-controls="site-nav"
        @click="menuOpen = !menuOpen"
      >
        <span></span>
      </button>

      <nav id="site-nav" class="nav" :class="{ 'is-open': menuOpen }">
        <a href="#solutions" @click="closeMenu">Our solutions</a>
        <a :href="PROFILE_URL" target="_blank" rel="noopener noreferrer" @click="closeMenu">
          Company profile
        </a>
        <a class="btn btn-teal" href="#enquiry" @click="closeMenu">Let's connect</a>
      </nav>
    </div>
  </header>

  <main id="top">
    <section class="wrap hero">
      <div class="hero-copy">
        <p class="eyebrow">The complete scientific ecosystem</p>
        <h1>
          Your next scientific
          <span>solution starts here.</span>
        </h1>
        <p class="lede">
          From routine consumables and specialized equipment to bioprocess solutions and complete
          projects, Neo Smart brings together the products, technologies and expertise your
          requirement needs.
        </p>
        <div class="hero-actions">
          <a class="btn btn-blue" href="#enquiry">
            Discuss your requirement
            <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
              <path d="M3 8h10M9 4l4 4-4 4" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" />
            </svg>
          </a>
          <a class="btn btn-line" :href="PROFILE_URL" target="_blank" rel="noopener noreferrer">
            View company profile
          </a>
        </div>
        <ul class="trust">
          <li>
            <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
              <path d="M8 1.5c2.2 2.4 3.5 4.4 3.5 6.3A3.5 3.5 0 1 1 4.5 7.8C4.5 5.9 5.8 3.9 8 1.5Z" fill="none" stroke="currentColor" stroke-width="1.4" />
              <circle cx="8" cy="8" r="1.2" fill="currentColor" />
            </svg>
            UAE based
          </li>
          <li>
            <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
              <rect x="2.2" y="3.2" width="11.6" height="10.2" rx="1.4" fill="none" stroke="currentColor" stroke-width="1.4" />
              <path d="M2.2 6.2h11.6M5.2 2.2v2M10.8 2.2v2" fill="none" stroke="currentColor" stroke-width="1.4" stroke-linecap="round" />
            </svg>
            Established 2020
          </li>
          <li>
            <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
              <circle cx="8" cy="8" r="5.6" fill="none" stroke="currentColor" stroke-width="1.4" />
              <path d="M2.6 8h10.8M8 2.4c1.6 1.6 2.4 3.5 2.4 5.6S9.6 12 8 13.6C6.4 12 5.6 10.1 5.6 8S6.4 4 8 2.4Z" fill="none" stroke="currentColor" stroke-width="1.3" />
            </svg>
            Global sourcing. Local support.
          </li>
        </ul>
      </div>

      <div class="hero-form">
        <form id="enquiry" class="card" novalidate @submit.prevent="prepareEmail">
          <p class="card-kicker">Let's discuss your requirement</p>
          <h2>Tell us what you need.</h2>

          <div class="fields">
            <label class="field" :class="{ 'is-invalid': nameInvalid }">
              <span>Full name <i class="req">*</i></span>
              <input v-model="form.name" type="text" name="name" autocomplete="name" placeholder="Your full name..." aria-required="true" />
            </label>
            <label class="field">
              <span>Company name</span>
              <input v-model="form.company" type="text" name="organization" autocomplete="organization" placeholder="Your company name..." />
            </label>
            <label class="field" :class="{ 'is-invalid': emailInvalid }">
              <span>Email address</span>
              <input v-model="form.email" type="email" name="email" autocomplete="email" placeholder="your@email.com" />
            </label>
            <label class="field" :class="{ 'is-invalid': phoneInvalid }">
              <span>WhatsApp / mobile</span>
              <input v-model="form.phone" type="tel" name="tel" autocomplete="tel" placeholder="Your mobile number..." />
            </label>
            <label class="field field-full" :class="{ 'is-invalid': requirementInvalid }">
              <span>Your requirement <i class="req">*</i></span>
              <textarea
                v-model="form.requirement"
                name="requirement"
                rows="4"
                placeholder="Tell us about your requirement, products, quantities, or any specific details..."
                aria-required="true"
              ></textarea>
            </label>
          </div>

          <p class="hint" :class="{ 'is-error': error }" role="status">
            {{ error || 'Please provide an email or mobile number.' }}
          </p>

          <div class="form-actions">
            <button class="btn btn-blue btn-block" type="submit">
              Prepare email enquiry
              <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
                <path d="M3 8h10M9 4l4 4-4 4" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" />
              </svg>
            </button>
            <p class="fine">Opens your email app with your enquiry ready to send.</p>
            <button class="btn btn-line btn-block" type="button" @click="chatWhatsApp">
              <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
                <path fill="currentColor" d="M8.1 1.4A6.5 6.5 0 0 0 2.3 11L1.5 14.4l3.5-.8A6.5 6.5 0 1 0 8.1 1.4Zm3.4 9.1c-.1.4-.8.7-1.1.8-.3 0-.6.1-2-.5-1.7-.8-2.8-2.5-2.9-2.6-.1-.1-.8-1-.8-1.9s.5-1.4.7-1.5h.5c.1 0 .3 0 .4.4.2.4.6 1.4.6 1.5.1.1 0 .3-.1.4l-.3.4c-.1.1-.2.2 0 .4.2.4.8 1.2 1.6 1.8.6.5 1.2.6 1.4.7.2 0 .3 0 .4-.1l.5-.6c.1-.2.3-.1.5-.1l1.2.6c.2.1.3.1.4.2.1.2 0 .6-.1.9Z" />
              </svg>
              Chat on WhatsApp
            </button>
          </div>

          <div class="legal">
            <p>
              <strong>Privacy policy.</strong>
              Details you submit are used only to respond to your enquiry. They are sent to
              info@neosmartgroup.com or WhatsApp and are not sold or shared for marketing.
            </p>
            <p>
              <strong>Disclaimer.</strong>
              This form is a request for information. It does not confirm availability, pricing, or an
              order. Neo Smart will confirm details after reviewing your requirement.
            </p>
          </div>
        </form>

        <div class="google-row">
          <span>Worked with Neo Smart?</span>
          <a class="google-link" :href="GOOGLE_URL" target="_blank" rel="noopener noreferrer">
            Rate us on Google
            <svg width="14" height="14" viewBox="0 0 16 16" aria-hidden="true">
              <path d="M4 12 12 4M7 4h5v5" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round" />
            </svg>
          </a>
        </div>
      </div>
    </section>

    <section id="solutions" class="wrap solutions">
      <h2>From vision to operation.</h2>
      <div class="solutions-grid">
        <article class="solution">
          <span class="sol-icon">
            <svg width="24" height="24" viewBox="0 0 24 24" aria-hidden="true">
              <circle cx="10" cy="10" r="4.2" fill="none" stroke="currentColor" stroke-width="1.7" />
              <path d="M13.2 13.2 18 19M8 10h4M10 8v4" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" />
            </svg>
          </span>
          <h3>Laboratory &amp; Scientific Equipment</h3>
        </article>
        <article class="solution">
          <span class="sol-icon">
            <svg width="24" height="24" viewBox="0 0 24 24" aria-hidden="true">
              <path d="M9 4h6M10 4v5.2L6.8 17a3.2 3.2 0 0 0 2.9 4.6h4.6A3.2 3.2 0 0 0 17.2 17L14 9.2V4" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" />
              <path d="M8.6 14.5h6.8" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" />
            </svg>
          </span>
          <h3>Bioprocess &amp; Production Solutions</h3>
        </article>
        <article class="solution">
          <span class="sol-icon">
            <svg width="24" height="24" viewBox="0 0 24 24" aria-hidden="true">
              <path d="M9 3h6l-1 7.2a4.5 4.5 0 1 1-4 0L9 3Z" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linejoin="round" />
              <path d="M9.5 14.5h5" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" />
            </svg>
          </span>
          <h3>Consumables &amp; Production Support</h3>
        </article>
        <article class="solution">
          <span class="sol-icon">
            <svg width="24" height="24" viewBox="0 0 24 24" aria-hidden="true">
              <path d="M5 10.5V19h14v-8.5M4 10.5h16M8 10.5V7.5A2.5 2.5 0 0 1 10.5 5h3A2.5 2.5 0 0 1 16 7.5v3M9 14.5h2.2M9 17h6" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" />
            </svg>
          </span>
          <h3>Laboratory Furniture &amp; Cleanroom Solutions</h3>
        </article>
        <article class="solution">
          <span class="sol-icon">
            <svg width="24" height="24" viewBox="0 0 24 24" aria-hidden="true">
              <circle cx="12" cy="12" r="2.2" fill="none" stroke="currentColor" stroke-width="1.7" />
              <path d="M12 4.5v2.2M12 17.3v2.2M4.5 12h2.2M17.3 12h2.2M6.7 6.7l1.6 1.6M15.7 15.7l1.6 1.6M17.3 6.7l-1.6 1.6M8.3 15.7l-1.6 1.6" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" />
            </svg>
          </span>
          <h3>Turnkey Project Delivery</h3>
        </article>
        <article class="solution">
          <span class="sol-icon">
            <svg width="24" height="24" viewBox="0 0 24 24" aria-hidden="true">
              <path d="M12 3.5 5.5 6v5.4c0 3.6 2.6 6.6 6.5 8.1 3.9-1.5 6.5-4.5 6.5-8.1V6L12 3.5Z" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linejoin="round" />
              <path d="m9 12 2 2 4-4.2" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" />
            </svg>
          </span>
          <h3>Calibration, Validation, Training &amp; Technical Support</h3>
        </article>
      </div>
    </section>
  </main>

  <footer class="site-footer">
    <div class="wrap footer-inner">
      <p class="footer-mark">Connecting science, technology &amp; innovation</p>
      <div class="footer-links">
        <a :href="PROFILE_URL" target="_blank" rel="noopener noreferrer">
          <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
            <path d="M4 2.5h5.2L13 6.2V13.5H4v-11Z" fill="none" stroke="currentColor" stroke-width="1.4" stroke-linejoin="round" />
            <path d="M9 2.6V6h3.3M6 9h5M6 11.2h5" fill="none" stroke="currentColor" stroke-width="1.4" stroke-linecap="round" />
          </svg>
          Company profile
        </a>
        <a :href="`https://wa.me/${PHONE_E164}`" target="_blank" rel="noopener noreferrer">
          <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
            <path fill="currentColor" d="M8.1 1.4A6.5 6.5 0 0 0 2.3 11L1.5 14.4l3.5-.8A6.5 6.5 0 1 0 8.1 1.4Zm3.4 9.1c-.1.4-.8.7-1.1.8-.3 0-.6.1-2-.5-1.7-.8-2.8-2.5-2.9-2.6-.1-.1-.8-1-.8-1.9s.5-1.4.7-1.5h.5c.1 0 .3 0 .4.4.2.4.6 1.4.6 1.5.1.1 0 .3-.1.4l-.3.4c-.1.1-.2.2 0 .4.2.4.8 1.2 1.6 1.8.6.5 1.2.6 1.4.7.2 0 .3 0 .4-.1l.5-.6c.1-.2.3-.1.5-.1l1.2.6c.2.1.3.1.4.2.1.2 0 .6-.1.9Z" />
          </svg>
          {{ PHONE_DISPLAY }}
        </a>
      </div>
    </div>
  </footer>
</template>

<style scoped>
#enquiry,
#solutions {
  scroll-margin-top: 92px;
}
</style>
