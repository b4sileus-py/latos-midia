document.documentElement.classList.add("js");

document.addEventListener("DOMContentLoaded", () => {
  const prefersReducedMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;

  initRevealOnScroll(prefersReducedMotion);
  initActiveNavigation();
  initBackToTopButton(prefersReducedMotion);
  initLightbox();
  initCurrentYear();
});

function initRevealOnScroll(prefersReducedMotion) {
  const revealItems = document.querySelectorAll(
    "main > section, .portfolio-item"
  );

  if (!revealItems.length) {
    return;
  }

  revealItems.forEach((item) => {
    item.classList.add("reveal-item");
  });

  if (
    prefersReducedMotion ||
    !("IntersectionObserver" in window)
  ) {
    revealItems.forEach((item) => {
      item.classList.add("is-visible");
    });

    return;
  }

  const revealObserver = new IntersectionObserver(
    (entries, observer) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add("is-visible");
          observer.unobserve(entry.target);
        }
      });
    },
    {
      threshold: 0.03,
      rootMargin: "0px 0px -5% 0px",
    }
  );

  revealItems.forEach((item) => {
    revealObserver.observe(item);
  });
}

function initActiveNavigation() {
  const header = document.querySelector(".site-header");
  const navLinks = Array.from(document.querySelectorAll(".main-nav a[href^='#']"));
  const sectionMap = new Map();

  navLinks.forEach((link) => {
    const sectionId = link.getAttribute("href").slice(1);
    const section = document.getElementById(sectionId);

    if (section) {
      sectionMap.set(section, link);
    }
  });

  if (!sectionMap.size || !("IntersectionObserver" in window)) {
    return;
  }

  const setActiveLink = (activeLink) => {
    navLinks.forEach((link) => {
      link.classList.toggle("is-active", link === activeLink);
    });
  };

  const headerHeight = header ? header.offsetHeight : 0;
  const navObserver = new IntersectionObserver(
    (entries) => {
      const visibleEntry = entries
        .filter((entry) => entry.isIntersecting)
        .sort((a, b) => b.intersectionRatio - a.intersectionRatio)[0];

      if (!visibleEntry) {
        return;
      }

      const activeLink = sectionMap.get(visibleEntry.target);
      if (activeLink) {
        setActiveLink(activeLink);
      }
    },
    {
      threshold: [0.2, 0.45, 0.7],
      rootMargin: `-${headerHeight + 16}px 0px -55% 0px`,
    }
  );

  sectionMap.forEach((_, section) => navObserver.observe(section));
}

function initBackToTopButton(prefersReducedMotion) {
  const backToTopButton = document.querySelector(".back-to-top");

  if (!backToTopButton) {
    return;
  }

  const toggleBackToTopButton = () => {
    const shouldShow = window.scrollY > 420;
    backToTopButton.classList.toggle("is-visible", shouldShow);
    backToTopButton.setAttribute("aria-hidden", String(!shouldShow));
    backToTopButton.tabIndex = shouldShow ? 0 : -1;
  };

  backToTopButton.setAttribute("aria-hidden", "true");
  toggleBackToTopButton();

  window.addEventListener("scroll", toggleBackToTopButton, { passive: true });

  backToTopButton.addEventListener("click", () => {
    window.scrollTo({
      top: 0,
      behavior: prefersReducedMotion ? "auto" : "smooth",
    });
  });
}

function initLightbox() {
  const lightbox = document.querySelector(".lightbox");
  const lightboxImage = document.querySelector(".lightbox-image");
  const lightboxClose = document.querySelector(".lightbox-close");
  const portfolioButtons = document.querySelectorAll(".portfolio-open");
  let activeTrigger = null;

  if (!lightbox || !lightboxImage || !lightboxClose || !portfolioButtons.length) {
    return;
  }

  const getFocusableElements = () =>
    Array.from(
      lightbox.querySelectorAll(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
      )
    ).filter((element) => !element.disabled && element.offsetParent !== null);

  const closeLightbox = () => {
    lightbox.hidden = true;
    lightbox.classList.remove("is-open");
    document.body.classList.remove("no-scroll");
    lightboxImage.removeAttribute("src");
    lightboxImage.alt = "";

    if (activeTrigger) {
      activeTrigger.focus();
      activeTrigger = null;
    }
  };

  const openLightbox = (button) => {
    const image = button.querySelector("img");

    if (!image) {
      return;
    }

    activeTrigger = button;
    lightboxImage.src = image.currentSrc || image.src;
    lightboxImage.alt = image.alt;
    lightbox.hidden = false;
    lightbox.classList.add("is-open");
    document.body.classList.add("no-scroll");
    lightboxClose.focus();
  };

  portfolioButtons.forEach((button) => {
    button.addEventListener("click", () => openLightbox(button));
  });

  lightboxClose.addEventListener("click", closeLightbox);

  lightbox.addEventListener("click", (event) => {
    const clickedOutsideImage =
      event.target === lightbox || event.target.classList.contains("lightbox-content");

    if (clickedOutsideImage) {
      closeLightbox();
    }
  });

  document.addEventListener("keydown", (event) => {
    if (lightbox.hidden) {
      return;
    }

    if (event.key === "Escape") {
      closeLightbox();
      return;
    }

    if (event.key !== "Tab") {
      return;
    }

    const focusableElements = getFocusableElements();
    const firstElement = focusableElements[0];
    const lastElement = focusableElements[focusableElements.length - 1];

    if (!firstElement || !lastElement) {
      event.preventDefault();
      lightboxClose.focus();
      return;
    }

    if (event.shiftKey && document.activeElement === firstElement) {
      event.preventDefault();
      lastElement.focus();
    } else if (!event.shiftKey && document.activeElement === lastElement) {
      event.preventDefault();
      firstElement.focus();
    }
  });
}

function initCurrentYear() {
  const currentYear = document.getElementById("current-year");

  if (currentYear) {
    currentYear.textContent = new Date().getFullYear();
  }
}
