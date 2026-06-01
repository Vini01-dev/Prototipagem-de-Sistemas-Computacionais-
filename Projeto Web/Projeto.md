[index.html](https://github.com/user-attachments/files/28446703/index.html)[booking.html](https://github.com/user-attachments/files/28446709/booking.html)

[style.css](https://github.com/user-attachments/files/28446713/style.css)
[home.html](https://github.com/user-attachments/files/28446706/home.html)[teachers.html](https://github.com/user-attachments/files/28446717/teachers.html)[login.js](https://github.com/user-attachments/files/[script.js](https://github.com/user-attachments/files/28446727/script.js)28446723/login.js)[booking.js](https://github.com/user-attachments/files/28446725/booking.js)
)<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />// Menu móvel
const menuToggle = document.querySelector('.menu-toggle');
const navLinks = document.querySelector('.nav-links');

if (menuToggle && navLinks) {
  menuToggle.addEventListener('click', () => {
    const isOpen = navLinks.classList.toggle('open');
    menuToggle.setAttribute('aria-expanded', String(isOpen));
  });
}

if (navLinks) {
  navLinks.querySelectorAll('a').forEach((link) => {
    link.addEventListener('click', () => {
      navLinks.classList.remove('open');
      if (menuToggle) {
        menuToggle.setAttribute('aria-expanded', 'false');
      }
    });
  });
}

// Revelar ao rolar
const revealEls = document.querySelectorAll('.section, .cta-section, .card, .feature, .impact, .ods');

revealEls.forEach((el) => el.classList.add('reveal'));

const io = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
        io.unobserve(entry.target);
      }
    });
  },
  {
    threshold: 0.12,
  }
);

revealEls.forEach((el) => io.observe(el));

// Formulário CTA
const form = document.getElementById('signupForm');
const msg = document.getElementById('ctaMsg');
const mentorSelect = document.getElementById('mentorSelect');
const mentorInfo = document.getElementById('mentorInfo');
const courseSelect = document.getElementById('courseSelect');
const mentorCourseSelect = document.getElementById('mentorCourseSelect');
const courseDesc = document.getElementById('courseDesc');
const courseMentorDesc = document.getElementById('courseMentorDesc');
const dateInput = document.getElementById('dateInput');
const timeInput = document.getElementById('timeInput');
const bookingInfo = document.getElementById('bookingInfo');

const mentorDescription = {
  academico: 'Mentor Acadêmico ajuda com estudo, rotina e apoio acadêmico.',
  carreira: 'Mentor de Carreira orienta escolhas profissionais, entrevistas e plano de carreira.',
  habilidades: 'Mentor de Habilidades foca em competências práticas como programação, design e comunicação.',
};

const courseData = {
  programacao: {
    title: 'Programação',
    description: 'Aprenda desenvolvimento web, lógica e projetos práticos em várias linguagens modernas.',
    mentors: [
      { value: 'marcos', name: 'Marcos Silva', description: 'Especialista em front-end e lógica de programação com abordagem prática.' },
      { value: 'carol', name: 'Carol Nascimento', description: 'Mentora experiente em back-end e desenvolvimento de aplicações reais.' },
    ],
  },
  ingles: {
    title: 'Inglês',
    description: 'Domine conversação, gramática e vocabulário para estudar ou trabalhar no exterior.',
    mentors: [
      { value: 'juliana', name: 'Juliana Costa', description: 'Professora de inglês com foco em fluência e preparação para intercâmbio.' },
      { value: 'rafael', name: 'Rafael Cruz', description: 'Especialista em inglês para negócios e comunicação profissional.' },
    ],
  },
  design: {
    title: 'Design',
    description: 'Aprenda criação visual, UX/UI e técnicas para transformar ideias em produtos digitais.',
    mentors: [
      { value: 'ana', name: 'Ana Souza', description: 'Designer com experiência em identidade visual e experiência do usuário.' },
      { value: 'pedro', name: 'Pedro Lima', description: 'Mentor em design digital e prototipagem de interfaces.' },
    ],
  },
  marketing: {
    title: 'Marketing Digital',
    description: 'Domine estratégias de vendas, redes sociais e presença online para empreendedores.',
    mentors: [
      { value: 'mariana', name: 'Mariana Dias', description: 'Especialista em marketing de conteúdo e crescimento de marcas.' },
      { value: 'felipe', name: 'Felipe Souza', description: 'Mentor em campanhas digitais e análise de métricas.' },
    ],
  },
  matematica: {
    title: 'Matemática',
    description: 'Reforço de raciocínio lógico, álgebra e preparação para provas de todos os níveis.',
    mentors: [
      { value: 'beatriz', name: 'Beatriz Rocha', description: 'Mentora em matemática básica e avançada para reforço escolar.' },
      { value: 'douglas', name: 'Douglas Menezes', description: 'Professor com foco em resolução de problemas e vestibulares.' },
    ],
  },
  empreendedorismo: {
    title: 'Empreendedorismo',
    description: 'Aprenda planejamento, modelagem de negócios e criação de projetos com impacto.',
    mentors: [
      { value: 'paula', name: 'Paula Martins', description: 'Mentora de startups e criação de negócios digitais sustentáveis.' },
      { value: 'thiago', name: 'Thiago Alves', description: 'Especialista em inovação e estratégia para novos empreendedores.' },
    ],
  },
};

function updateMentorInfo() {
  if (mentorSelect && mentorInfo) {
    mentorInfo.textContent = mentorDescription[mentorSelect.value] || mentorDescription.academico;
  }
}

function populateMentorCourseOptions() {
  if (!courseSelect || !mentorCourseSelect) return;

  const course = courseData[courseSelect.value] || courseData.programacao;
  mentorCourseSelect.innerHTML = '';

  course.mentors.forEach((mentor) => {
    const option = document.createElement('option');
    option.value = mentor.value;
    option.textContent = mentor.name;
    mentorCourseSelect.appendChild(option);
  });

  updateCourseDescription();
  updateMentorCourseDescription();
  updateBookingInfo();
}

function updateCourseDescription() {
  if (!courseSelect || !courseDesc) return;

  const course = courseData[courseSelect.value] || courseData.programacao;
  courseDesc.textContent = course.description;
}

function updateMentorCourseDescription() {
  if (!courseSelect || !mentorCourseSelect || !courseMentorDesc) return;

  const course = courseData[courseSelect.value] || courseData.programacao;
  const mentor = course.mentors.find((item) => item.value === mentorCourseSelect.value) || course.mentors[0];
  courseMentorDesc.textContent = `${mentor.name} — ${mentor.description}`;
}

function updateBookingInfo() {
  if (!courseSelect || !mentorCourseSelect || !bookingInfo) return;

  const course = courseData[courseSelect.value] || courseData.programacao;
  const mentor = course.mentors.find((item) => item.value === mentorCourseSelect.value) || course.mentors[0];
  const date = dateInput?.value || 'não selecionada';
  const time = timeInput?.value || 'não selecionado';

  bookingInfo.textContent = `Curso: ${course.title}. Mentor: ${mentor.name}. Data: ${date}. Horário: ${time}.`;
}

if (mentorSelect && mentorInfo) {
  mentorSelect.addEventListener('change', updateMentorInfo);
  updateMentorInfo();
}

if (courseSelect) {
  courseSelect.addEventListener('change', populateMentorCourseOptions);
}

if (mentorCourseSelect) {
  mentorCourseSelect.addEventListener('change', () => {
    updateMentorCourseDescription();
    updateBookingInfo();
  });
}

const confirmBookingBtn = document.getElementById('confirmBooking');
const bookingConfirmation = document.getElementById('bookingConfirmation');

if (dateInput) {
  dateInput.addEventListener('change', updateBookingInfo);
}

if (timeInput) {
  timeInput.addEventListener('change', updateBookingInfo);
}

if (confirmBookingBtn && bookingInfo && bookingConfirmation) {
  confirmBookingBtn.addEventListener('click', () => {
    const course = courseData[courseSelect.value] || courseData.programacao;
    const mentor = course.mentors.find((item) => item.value === mentorCourseSelect.value) || course.mentors[0];
    const date = dateInput?.value;
    const time = timeInput?.value;

    if (!date || !time) {
      bookingConfirmation.textContent = 'Por favor, escolha uma data e um horário para confirmar o agendamento.';
      bookingConfirmation.style.color = '#ffb86c';
      return;
    }

    bookingConfirmation.textContent = `Agendamento confirmado! ${mentor.name} estará disponível em ${date} às ${time} no curso de ${course.title}.`;    
    bookingConfirmation.style.color = '#50fa7b';
    bookingInfo.textContent = `Curso: ${course.title}. Mentor: ${mentor.name}. Data: ${date}. Horário: ${time}.`;
  });
}

populateMentorCourseOptions();
updateMentorInfo();

if (form && msg) {
  form.addEventListener('submit', (event) => {
    event.preventDefault();

    const emailInput = form.querySelector('input[type="email"]');
    const email = emailInput?.value.trim() || '';

    if (!email) {
      msg.textContent = 'Por favor, digite um e-mail válido.';
      return;
    }

    msg.textContent = `Obrigado! Em breve entraremos em contato em ${email}.`;
    form.reset();
  });
}

  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Agendamentos — EduConnect</title>
  <meta name="description" content="Página de agendamentos do EduConnect. Agende um mentor, confirme horário e avalie o curso" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <header class="navbar">
    <div class="container nav-inner">
      <a href="home.html" class="logo">
        <span class="logo-mark">Edu</span><span>Connect</span>
      </a>
      <nav aria-label="Menu principal">
        <ul class="nav-links">
          <li><a href="home.html">Home</a></li>
          <li><a href="teachers.html">Professores</a></li>
          <li><a href="booking.html">Agendamentos</a></li>
          <li><a href="#booking">Agendar</a></li>
          <li><a href="#avaliacoes">Avaliações</a></li>
        </ul>
      </nav>
      <button class="menu-toggle" aria-label="Abrir menu" aria-expanded="false">☰</button>
    </div>
  </header>

  <main>
    <section class="hero">
      <div class="container hero-inner">
        <div class="hero-text">
          <span class="tag">Agendamentos • Mentoria • Avaliação</span>
          <h1>Agende seu encontro com o mentor ideal</h1>
          <p>Nesta página você pode escolher o curso, o mentor, data e horário, além de avaliar sua experiência ao final do atendimento.</p>
          <div class="hero-cta">
            <a href="#booking" class="btn btn-primary">Agendar agora</a>
            <a href="#avaliacoes" class="btn btn-ghost">Avaliar mentor</a>
          </div>
        </div>
      </div>
    </section>

    <section id="booking" class="section">
      <div class="container">
        <span class="eyebrow">01 — Agendamento</span>
        <h2>Marque seu horário com um mentor</h2>
        <p class="lead">Escolha o curso e o mentor disponível, informe dia e hora, e confirme o seu encontro.</p>

        <div class="booking-section">
          <div class="booking-grid">
            <div class="booking-card">
              <label for="bookingCourse">Curso desejado</label>
              <select id="bookingCourse" class="mentor-select">
                <option value="programacao">Programação</option>
                <option value="ingles">Inglês</option>
                <option value="design">Design</option>
                <option value="marketing">Marketing Digital</option>
                <option value="matematica">Matemática</option>
                <option value="empreendedorismo">Empreendedorismo</option>
              </select>
            </div>
            <div class="booking-card">
              <label for="bookingMentor">Mentor disponível</label>
              <select id="bookingMentor" class="mentor-select"></select>
            </div>
            <div class="booking-card">
              <label for="bookingLevel">Nível da mentoria</label>
              <select id="bookingLevel" class="mentor-select">
                <option value="Basico">Básico</option>
                <option value="Intermediario">Intermediário</option>
                <option value="Avancado">Avançado</option>
              </select>
            </div>
            <div class="booking-card">
              <label for="bookingDate">Data</label>
              <input id="bookingDate" type="date" />
            </div>
            <div class="booking-card">
              <label for="bookingTime">Horário</label>
              <input id="bookingTime" type="time" />
            </div>
          </div>

          <div class="booking-summary">
            <div>
              <h3>Resumo do agendamento</h3>
              <p id="bookingSummary">Escolha curso, mentor, data e horário para gerar o resumo.</p>
            </div>
            <div class="booking-confirmation-wrapper">
              <button id="bookingConfirmButton" class="btn btn-primary" type="button">Confirmar agendamento</button>
              <p id="bookingResult" class="booking-confirmation-message"></p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section id="avaliacoes" class="section section-alt">
      <div class="container">
        <span class="eyebrow">02 — Avaliações</span>
        <h2>Avalie seu mentor e o curso concluído</h2>
        <p class="lead">Deixe sua opinião sobre o mentor escolhido e sobre o curso que você concluiu.</p>

        <div class="booking-section">
          <div class="booking-grid">
            <div class="booking-card">
              <label for="reviewMentor">Mentor avaliado</label>
              <select id="reviewMentor" class="mentor-select"></select>
            </div>
            <div class="booking-card">
              <label>Nota do mentor</label>
              <div id="mentorRatingStars" class="star-rating" role="radiogroup" aria-label="Avaliação do mentor">
                <button type="button" class="star" data-value="1" aria-label="1 estrela">★</button>
                <button type="button" class="star" data-value="2" aria-label="2 estrelas">★</button>
                <button type="button" class="star" data-value="3" aria-label="3 estrelas">★</button>
                <button type="button" class="star" data-value="4" aria-label="4 estrelas">★</button>
                <button type="button" class="star" data-value="5" aria-label="5 estrelas">★</button>
              </div>
              <span id="mentorRatingText" class="rating-text">0/5 estrelas</span>
            </div>
            <div class="booking-card">
              <label>Nota do curso</label>
              <div id="courseRatingStars" class="star-rating" role="radiogroup" aria-label="Avaliação do curso">
                <button type="button" class="star" data-value="1" aria-label="1 estrela">★</button>
                <button type="button" class="star" data-value="2" aria-label="2 estrelas">★</button>
                <button type="button" class="star" data-value="3" aria-label="3 estrelas">★</button>
                <button type="button" class="star" data-value="4" aria-label="4 estrelas">★</button>
                <button type="button" class="star" data-value="5" aria-label="5 estrelas">★</button>
              </div>
              <span id="courseRatingText" class="rating-text">0/5 estrelas</span>
            </div>
          </div>

          <div class="booking-card">
            <label for="reviewComments">Comentário</label>
            <textarea id="reviewComments" rows="5" placeholder="Conte como foi a mentoria e o curso"></textarea>
          </div>

          <div class="booking-summary">
            <button id="submitReview" class="btn btn-ghost" type="button">Enviar avaliação</button>
            <p id="reviewResult" class="booking-confirmation-message"></p>
          </div>
        </div>
      </div>
    </section>
  </main>

  <footer class="footer">
    <div class="container footer-inner">
      <div>
        <a href="home.html" class="logo">
          <span class="logo-mark">Edu</span><span>Connect</span>
        </a>
        <p>Volte à página inicial sempre que quiser reservar outra mentoria.</p>
      </div>
      <div class="footer-links">
        <a href="home.html">Home</a>
        <a href="booking.html">Agendamentos</a>
      </div>
      <p class="copy">© 2026 EduConnect. Todos os direitos reservados.</p>
    </div>
  </footer>

  <script src="booking.js"></script>
</body>
</html>
