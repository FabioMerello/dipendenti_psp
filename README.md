/* =========================================================
   P&P SPORT MANAGEMENT — PORTALE INTERNO
   JS vanilla — nessuna dipendenza esterna
   ========================================================= */
(() => {
  'use strict';

  const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  /* ---------- Anno corrente nel footer ---------- */
  const yearEl = document.getElementById('current-year');
  if (yearEl) yearEl.textContent = new Date().getFullYear();

  /* ---------- Orologio / data "live" nella dashboard ---------- */
  const clockEl = document.getElementById('live-clock');
  if (clockEl) {
    const formatter = new Intl.DateTimeFormat('it-IT', {
      hour: '2-digit',
      minute: '2-digit',
      second: '2-digit',
      day: '2-digit',
      month: '2-digit',
      year: 'numeric'
    });
    const tick = () => { clockEl.textContent = formatter.format(new Date()); };
    tick();
    setInterval(tick, 1000);
  }

  /* ---------- Menu mobile ---------- */
  const navToggle = document.querySelector('.nav-toggle');
  const nav = document.getElementById('primary-nav');
  if (navToggle && nav) {
    navToggle.addEventListener('click', () => {
      const isOpen = nav.classList.toggle('is-open');
      navToggle.setAttribute('aria-expanded', String(isOpen));
    });
    nav.querySelectorAll('a').forEach(link => {
      link.addEventListener('click', () => {
        nav.classList.remove('is-open');
        navToggle.setAttribute('aria-expanded', 'false');
      });
    });
  }

  /* ---------- Header: leggero cambio sfondo on scroll ---------- */
  const header = document.querySelector('.site-header');
  if (header) {
    const onScroll = () => {
      header.style.background = window.scrollY > 12
        ? 'rgba(10,20,40,.92)'
        : 'rgba(10,20,40,.72)';
    };
    document.addEventListener('scroll', onScroll, { passive: true });
    onScroll();
  }

  /* ---------- Reveal on scroll ---------- */
  const revealEls = document.querySelectorAll('.reveal');
  if ('IntersectionObserver' in window && !reduceMotion) {
    const io = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          entry.target.classList.add('is-visible');
          io.unobserve(entry.target);
        }
      });
    }, { threshold: 0.15, rootMargin: '0px 0px -8% 0px' });
    revealEls.forEach(el => io.observe(el));
  } else {
    revealEls.forEach(el => el.classList.add('is-visible'));
  }

  /* ---------- Stagger automatico per liste con .reveal-stagger ---------- */
  document.querySelectorAll('.reveal-stagger').forEach(group => {
    Array.from(group.children).forEach((child, i) => {
      child.style.setProperty('--i', i);
      child.classList.add('reveal');
    });
  });

  /* ---------- Contatori numerici animati ---------- */
  const counters = document.querySelectorAll('[data-counter]');

  function easeOutExpo(t) {
    return t === 1 ? 1 : 1 - Math.pow(2, -10 * t);
  }

  function animateCounter(el) {
    const target = parseFloat(el.getAttribute('data-counter'));
    const decimals = parseInt(el.getAttribute('data-decimals') || '0', 10);
    const duration = parseInt(el.getAttribute('data-duration') || '1800', 10);
    const locale = 'it-IT';

    if (reduceMotion) {
      el.textContent = target.toLocaleString(locale, {
        minimumFractionDigits: decimals,
        maximumFractionDigits: decimals
      });
      return;
    }

    const start = performance.now();

    function frame(now) {
      const elapsed = now - start;
      const progress = Math.min(elapsed / duration, 1);
      const eased = easeOutExpo(progress);
      const value = target * eased;
      el.textContent = value.toLocaleString(locale, {
        minimumFractionDigits: decimals,
        maximumFractionDigits: decimals
      });
      if (progress < 1) {
        requestAnimationFrame(frame);
      } else {
        el.textContent = target.toLocaleString(locale, {
          minimumFractionDigits: decimals,
          maximumFractionDigits: decimals
        });
      }
    }
    requestAnimationFrame(frame);
  }

  if ('IntersectionObserver' in window) {
    const counterIO = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        if (entry.isIntersecting) {
          animateCounter(entry.target);
          counterIO.unobserve(entry.target);
        }
      });
    }, { threshold: 0.6 });
    counters.forEach(c => counterIO.observe(c));
  } else {
    counters.forEach(animateCounter);
  }

  /* ---------- Evidenzia voce TOC attiva nella pagina legale ---------- */
  const tocLinks = document.querySelectorAll('.legal-toc a');
  const sections = Array.from(tocLinks)
    .map(link => document.querySelector(link.getAttribute('href')))
    .filter(Boolean);

  if (tocLinks.length && sections.length && 'IntersectionObserver' in window) {
    const tocIO = new IntersectionObserver((entries) => {
      entries.forEach(entry => {
        const id = '#' + entry.target.id;
        const link = document.querySelector(`.legal-toc a[href="${id}"]`);
        if (!link) return;
        if (entry.isIntersecting) {
          tocLinks.forEach(l => l.style.color = '');
          tocLinks.forEach(l => l.style.borderColor = 'transparent');
          link.style.color = 'var(--ink)';
          link.style.borderColor = 'var(--silver)';
        }
      });
    }, { rootMargin: '-20% 0px -70% 0px' });
    sections.forEach(s => tocIO.observe(s));
  }

})();
