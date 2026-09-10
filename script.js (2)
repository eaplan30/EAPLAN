window.addEventListener('DOMContentLoaded', () => {
    const savedUser = localStorage.getItem('ea_plan_active_user');
    
    setTimeout(() => {
        const splash = document.getElementById('splash-screen');
        const welcomeScreen = document.getElementById('welcome-screen');
        const homeScreen = document.getElementById('home-screen');

        splash.style.opacity = '0';
        setTimeout(() => {
            splash.style.display = 'none';
            if (savedUser) {
                document.getElementById('user-greeting').textContent = savedUser;
                loadProfileData(savedUser);
                homeScreen.style.display = 'flex';
                loadSeriesOptions();
                renderCreatorSeries();
            } else {
                welcomeScreen.style.display = 'flex';
            }
        }, 600);
    }, 2000);
});

function goToLogin() {
    const welcomeScreen = document.getElementById('welcome-screen');
    const authScreen = document.getElementById('auth-screen');
    welcomeScreen.style.opacity = '0';
    setTimeout(() => {
        welcomeScreen.style.display = 'none';
        authScreen.style.display = 'flex';
        switchTab('login');
    }, 400);
}

function handleLogin(event) {
    event.preventDefault();
    const email = document.getElementById('login-email').value.trim();
    const password = document.getElementById('login-password').value;
    
    const registeredName = localStorage.getItem('user_' + email.toLowerCase());
    if (!registeredName) {
        alert("Email belum terdaftar! Silakan lakukan Signup terlebih dahulu.");
        return;
    }

    const registeredPass = localStorage.getItem('pass_' + email.toLowerCase());
    if (registeredPass && registeredPass !== password) {
        alert("Password salah!");
        return;
    }

    localStorage.setItem('ea_plan_active_user', registeredName);
    document.getElementById('user-greeting').textContent = registeredName;
    loadProfileData(registeredName);
    loadSeriesOptions();
    renderCreatorSeries();
    transitionToDashboard();
}

function handleSignup(event) {
    event.preventDefault();
    const name = document.getElementById('signup-name').value;
    const email = document.getElementById('signup-email').value;
    const password = document.getElementById('signup-password').value;
    const errorBox = document.getElementById('error-message');

    if (!/^[A-Z]/.test(name)) {
        showError("Nama lengkap harus diawali dengan huruf kapital!");
        return;
    }
    if (!/^[A-Z].*(?=\d)(?=.*[\W_]).*$/.test(email) || !email.includes('@')) {
        showError("Email signup harus diawali huruf kapital, mengandung angka, dan simbol!");
        return;
    }
    if (!/^[A-Z].*(?=\d)(?=.*[\W_]).*$/.test(password)) {
        showError("Password harus diawali huruf kapital, mengandung angka, dan simbol!");
        return;
    }

    const emailKey = email.toLowerCase();
    localStorage.setItem('user_' + emailKey, name);
    localStorage.setItem('pass_' + emailKey, password);
    localStorage.setItem('ea_plan_active_user', name);

    localStorage.setItem('profile_username_' + name, '@' + name.toLowerCase().replace(/\s+/g, '_') + '_');
    localStorage.setItem('profile_badge_' + name, 'Penulis');
    localStorage.setItem('profile_bio_' + name, 'Selayaknya manusia, kita memiliki rasa perlu dan tidak perlu untuk hal tentang menanggapi');
    localStorage.setItem('profile_followers_' + name, '3');
    localStorage.setItem('user_email_' + name, email);

    errorBox.style.display = 'none';
    document.getElementById('user-greeting').textContent = name;
    loadProfileData(name);
    loadSeriesOptions();
    renderCreatorSeries();
    transitionToDashboard();
}

function socialLogin(provider) {
    const socialUserName = "Pengguna " + provider;
    localStorage.setItem('ea_plan_active_user', socialUserName);
    document.getElementById('user-greeting').textContent = socialUserName;
    loadProfileData(socialUserName);
    loadSeriesOptions();
    renderCreatorSeries();
    transitionToDashboard();
}

let temporaryAvatarBase64 = "";
let temporaryCoverBase64 = "";
let temporarySeriCoverBase64 = "";
let currentActiveSeriesTitle = "Yah";

function previewUploadedImage(event) {
    const file = event.target.files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
            temporaryAvatarBase64 = e.target.result;
            document.getElementById('modal-avatar-preview').src = temporaryAvatarBase64;
        };
        reader.readAsDataURL(file);
    }
}

function previewCoverImage(event) {
    const file = event.target.files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
            temporaryCoverBase64 = e.target.result;
            const container = document.getElementById('cover-preview-container');
            container.innerHTML = `
                <img src="${temporaryCoverBase64}" alt="Cover Preview" style="width: 100%; height: 160px; object-fit: cover; border-radius: 6px;">
                <span style="font-size: 11px; color: #555; margin-top: 6px; font-weight: 600;">Klik untuk mengganti cover</span>
            `;
        };
        reader.readAsDataURL(file);
    }
}

function previewSeriCoverImage(event) {
    const file = event.target.files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
            temporarySeriCoverBase64 = e.target.result;
            const container = document.getElementById('seri-cover-preview-container');
            container.innerHTML = `
                <img src="${temporarySeriCoverBase64}" alt="Seri Cover Preview" style="width: 100%; height: 160px; object-fit: cover; border-radius: 6px;">
                <span style="font-size: 11px; color: #555; margin-top: 6px; font-weight: 600;">Klik untuk mengganti cover</span>
            `;
        };
        reader.readAsDataURL(file);
    }
}

function loadProfileData(username) {
    const savedName = username;
    const savedUsername = localStorage.getItem('profile_username_' + savedName) || ('@' + savedName.toLowerCase().replace(/\s+/g, '_') + '_');
    const savedBadge = localStorage.getItem('profile_badge_' + savedName) || 'Penulis';
    const savedBio = localStorage.getItem('profile_bio_' + savedName) || 'Selayaknya manusia, kita memiliki rasa perlu dan tidak perlu untuk hal tentang menanggapi';
    const savedFollowers = localStorage.getItem('profile_followers_' + savedName) || '3';
    const savedAvatar = localStorage.getItem('profile_avatar_' + savedName) || 'logo.jpg';
    const savedEmail = localStorage.getItem('user_email_' + savedName) || (savedName.toLowerCase().replace(/\s+/g, '') + '@gmail.com');

    document.getElementById('main-profile-name').textContent = savedName.toUpperCase();
    document.getElementById('main-profile-email').textContent = savedEmail;
    document.getElementById('main-profile-avatar-img').src = savedAvatar;

    document.getElementById('creator-header-name').textContent = savedName.toUpperCase();
    document.getElementById('creator-profile-name').textContent = savedName.toUpperCase();
    document.getElementById('creator-profile-username').textContent = savedUsername;
    document.getElementById('creator-badge-display').textContent = savedBadge;
    document.getElementById('creator-about-name').textContent = savedName.toUpperCase();
    document.getElementById('creator-bio-display').textContent = savedBio;
    document.getElementById('stat-followers-count').textContent = savedFollowers;
    document.getElementById('stat-followers-val').textContent = savedFollowers;
    document.getElementById('creator-avatar-img-view').src = savedAvatar;
}

function openEditProfileModal() {
    const activeUser = localStorage.getItem('ea_plan_active_user') || "DINAR KARSA";
    document.getElementById('edit-name-input').value = activeUser;
    document.getElementById('edit-username-input').value = localStorage.getItem('profile_username_' + activeUser) || ('@' + activeUser.toLowerCase().replace(/\s+/g, '_') + '_');
    document.getElementById('edit-badge-input').value = localStorage.getItem('profile_badge_' + activeUser) || 'Penulis';
    document.getElementById('edit-bio-input').value = localStorage.getItem('profile_bio_' + activeUser) || '';
    
    const currentAvatar = localStorage.getItem('profile_avatar_' + activeUser) || 'logo.jpg';
    document.getElementById('modal-avatar-preview').src = currentAvatar;
    temporaryAvatarBase64 = currentAvatar;

    document.getElementById('edit-profile-modal').style.display = 'flex';
}

function closeEditProfileModal() {
    document.getElementById('edit-profile-modal').style.display = 'none';
}

function saveProfileChanges() {
    const newName = document.getElementById('edit-name-input').value.trim();
    const newUsername = document.getElementById('edit-username-input').value.trim();
    const newBadge = document.getElementById('edit-badge-input').value.trim();
    const newBio = document.getElementById('edit-bio-input').value.trim();

    if (!newName) {
        alert("Nama tidak boleh kosong!");
        return;
    }

    localStorage.setItem('ea_plan_active_user', newName);
    localStorage.setItem('user_' + newName.toLowerCase(), newName);

    localStorage.setItem('profile_username_' + newName, newUsername);
    localStorage.setItem('profile_badge_' + newName, newBadge);
    localStorage.setItem('profile_bio_' + newName, newBio);
    if (temporaryAvatarBase64) {
        localStorage.setItem('profile_avatar_' + newName, temporaryAvatarBase64);
    }

    loadProfileData(newName);
    document.getElementById('user-greeting').textContent = newName;
    closeEditProfileModal();
    alert("Profil berhasil diperbarui!");
}

function toggleProfileMenu() {
    const sidebar = document.getElementById('profile-sidebar');
    sidebar.classList.toggle('active');
}

function openAboutModal() {
    toggleProfileMenu();
    document.getElementById('about-modal').style.display = 'flex';
}

function closeAboutModal() {
    document.getElementById('about-modal').style.display = 'none';
}

function switchNav(event, menuName) {
    event.preventDefault();
    const navItems = document.querySelectorAll('.home-bottom-bar .home-nav-item');
    navItems.forEach(item => item.classList.remove('active'));
    event.currentTarget.classList.add('active');

    document.getElementById('tab-content-home').style.display = 'none';
    document.getElementById('tab-content-roadmap').style.display = 'none';
    document.getElementById('tab-content-journal').style.display = 'none';
    document.getElementById('tab-content-kategori-karya').style.display = 'none';
    document.getElementById('tab-content-detail-seri').style.display = 'none';
    document.getElementById('tab-content-view-detail-seri').style.display = 'none';
    document.getElementById('tab-content-detail-karya').style.display = 'none';
    document.getElementById('tab-content-isi-karya').style.display = 'none';
    document.getElementById('tab-content-review-karya').style.display = 'none';
    document.getElementById('tab-content-my-journal').style.display = 'none';
    document.getElementById('tab-content-stats').style.display = 'none';
    document.getElementById('tab-content-profile').style.display = 'none';
    document.getElementById('tab-content-creator-view').style.display = 'none';

    if (menuName === 'home') {
        document.getElementById('tab-content-home').style.display = 'flex';
    } else if (menuName === 'roadmap') {
        document.getElementById('tab-content-roadmap').style.display = 'flex';
    } else if (menuName === 'journal') {
        document.getElementById('tab-content-journal').style.display = 'flex';
    } else if (menuName === 'stats') {
        document.getElementById('tab-content-stats').style.display = 'flex';
    } else if (menuName === 'profile') {
        const activeUser = localStorage.getItem('ea_plan_active_user') || "DINAR KARSA";
        loadProfileData(activeUser);
        document.getElementById('tab-content-profile').style.display = 'flex';
    }
}

function openKategoriKaryaSection() {
    document.getElementById('tab-content-journal').style.display = 'none';
    document.getElementById('tab-content-kategori-karya').style.display = 'flex';
}

function backToJournalMenu() {
    document.getElementById('tab-content-kategori-karya').style.display = 'none';
    document.getElementById('tab-content-journal').style.display = 'flex';
}

/* --- DETAIL SERI NAVIGASI & PENYIMPANAN --- */
function openDetailSeriSection() {
    document.getElementById('seri-judul-input').value = 'Yah';
    document.getElementById('seri-sinopsis-textarea').value = 'Ya';
    document.getElementById('seri-genre-select').value = 'Romansa';
    document.getElementById('seri-status-select').value = 'Berlanjut';
    temporarySeriCoverBase64 = "";
    document.getElementById('seri-cover-preview-container').innerHTML = `
        <span class="material-icons" style="font-size: 28px; color: #888; margin-bottom: 4px;">cloud_upload</span>
        <span class="dk-upload-title">Unggah Cover (direkomendasikan)</span>
        <span class="dk-upload-desc">Rekomendasi ukuran 600:900 (4:6) atau lebih.</span>
    `;

    document.getElementById('tab-content-journal').style.display = 'none';
    document.getElementById('tab-content-detail-karya').style.display = 'none';
    document.getElementById('tab-content-detail-seri').style.display = 'flex';
}

function backToJournalFromSeri() {
    document.getElementById('tab-content-detail-seri').style.display = 'none';
    document.getElementById('tab-content-journal').style.display = 'flex';
}

function saveDetailSeri() {
    const title = document.getElementById('seri-judul-input').value.trim() || "Yah";
    const synopsis = document.getElementById('seri-sinopsis-textarea').value.trim() || "Ya";
    const genre = document.getElementById('seri-genre-select').value || "Romansa";
    const status = document.getElementById('seri-status-select').value || "Berlanjut";

    currentActiveSeriesTitle = title;

    const activeUser = localStorage.getItem('ea_plan_active_user') || "default_user";
    const seriesKey = 'user_series_detailed_' + activeUser;
    let seriesList = JSON.parse(localStorage.getItem(seriesKey)) || [];

    const newSeri = {
        id: Date.now(),
        title: title,
        synopsis: synopsis,
        genre: genre,
        status: status,
        cover: temporarySeriCoverBase64 || "",
        works: []
    };

    seriesList.unshift(newSeri);
    localStorage.setItem(seriesKey, JSON.stringify(seriesList));

    const simpleSeriesKey = 'user_series_' + activeUser;
    let simpleList = JSON.parse(localStorage.getItem(simpleSeriesKey)) || ['Kekasih Pertama & Terakhir', 'Zona Nyaman'];
    if (!simpleList.includes(title)) {
        simpleList.push(title);
        localStorage.setItem(simpleSeriesKey, JSON.stringify(simpleList));
    }

    loadSeriesOptions();
    renderCreatorSeries();

    // Langsung menuju ke halaman tampilan detail seri (Persis Referensi Foto 1000051016.png)
    document.getElementById('view-seri-title').textContent = title;
    document.getElementById('view-seri-synopsis').textContent = synopsis;
    const badge = document.getElementById('view-seri-status-badge');
    badge.textContent = status;
    badge.className = status === 'Berlanjut' ? 'berlanjut-badge' : 'berlanjut-badge';

    renderSeriesWorksContainer(newSeri.works);

    document.getElementById('tab-content-detail-seri').style.display = 'none';
    document.getElementById('tab-content-view-detail-seri').style.display = 'flex';
}

function backToJournalFromSeriView() {
    document.getElementById('tab-content-view-detail-seri').style.display = 'none';
    document.getElementById('tab-content-journal').style.display = 'flex';
}

/* --- MODAL PILIH / TAMBAH KARYA KE SERI --- */
function openAddWorkModal() {
    document.getElementById('add-work-to-series-modal').style.display = 'flex';
}

function closeAddWorkModal() {
    document.getElementById('add-work-to-series-modal').style.display = 'none';
}

function linkExistingWork() {
    const selectedWork = document.getElementById('existing-work-select').value;
    if (!selectedWork) {
        alert("Pilih karya yang sudah ada terlebih dahulu!");
        return;
    }
    closeAddWorkModal();
    addWorkToActiveSeries(selectedWork);
}

function createNewWorkForSeries() {
    closeAddWorkModal();
    // Buka form pembuatan kategori karya baru, dengan seri otomatis terpilih
    document.getElementById('tab-content-view-detail-seri').style.display = 'none';
    document.getElementById('tab-content-detail-karya').style.display = 'flex';
    document.getElementById('karya-series-select').value = currentActiveSeriesTitle;
}

function addWorkToActiveSeries(workName) {
    const activeUser = localStorage.getItem('ea_plan_active_user') || "default_user";
    const seriesKey = 'user_series_detailed_' + activeUser;
    let seriesList = JSON.parse(localStorage.getItem(seriesKey)) || [];

    if (seriesList.length > 0) {
        seriesList[0].works.push(workName);
        localStorage.setItem(seriesKey, JSON.stringify(seriesList));
        renderSeriesWorksContainer(seriesList[0].works);
    }
    alert(`Karya "${workName}" berhasil ditambahkan ke seri!`);
}

function renderSeriesWorksContainer(worksArray) {
    const container = document.getElementById('seri-works-container');
    if (!worksArray || worksArray.length === 0) {
        container.innerHTML = `
            <p>Kamu belum punya karya, yuk mulai karyamu dari sini.</p>
            <button class="btn-tambah-karya-seri" onclick="openAddWorkModal()">Tambah Karya</button>
        `;
    } else {
        let html = `<h4 style="font-size:14px; font-weight:700; align-self:flex-start; margin-bottom:4px;">Daftar Karya dalam Seri</h4>`;
        worksArray.forEach(w => {
            html += `<div style="background:#faf8f5; padding:10px 14px; border-radius:6px; width:100%; display:flex; justify-content:space-between; align-items:center; font-size:13px; font-weight:600;"><span>📖 ${escapeHtml(w)}</span><span style="color:#ff3366; font-size:11px; cursor:pointer;" onclick="alert('Membuka detail karya')">Kelola</span></div>`;
        });
        html += `<button class="btn-tambah-karya-seri" style="margin-top:10px;" onclick="openAddWorkModal()">+ Tambah Karya Lain</button>`;
        container.innerHTML = html;
    }
}

function renderCreatorSeries() {
    const container = document.getElementById('creator-series-grid-container');
    if (!container) return;

    const activeUser = localStorage.getItem('ea_plan_active_user') || "default_user";
    const seriesKey = 'user_series_detailed_' + activeUser;
    let seriesList = JSON.parse(localStorage.getItem(seriesKey)) || [];

    let defaultHTML = `
        <div class="book-card" onclick="openExistingSeriesView('Yah', 'Ya', 'Berlanjut')">
            <div class="book-cover-wrap book-bg-1"><span class="book-title-overlay">Yah</span></div>
            <p class="book-name">Yah</p>
        </div>
        <div class="book-card">
            <div class="book-cover-wrap book-bg-2"><span class="book-title-overlay">ZONA NYAMAN</span></div>
            <p class="book-name">Zona Nyaman</p>
        </div>
    `;

    let customHTML = '';
    seriesList.forEach(item => {
        let coverStyle = item.cover ? `background: url('${item.cover}') center/cover no-repeat;` : `background: linear-gradient(135deg, #b89768, #5a4a32);`;
        customHTML += `
            <div class="book-card" onclick="openExistingSeriesView('${escapeHtml(item.title)}', '${escapeHtml(item.synopsis)}', '${escapeHtml(item.status)}')">
                <div class="book-cover-wrap" style="${coverStyle}"><span class="book-title-overlay">${escapeHtml(item.title)}</span></div>
                <p class="book-name">${escapeHtml(item.title)}</p>
            </div>
        `;
    });

    container.innerHTML = customHTML + defaultHTML;
    document.getElementById('creator-series-count-label').textContent = `${seriesList.length + 2} seri telah dibuat`;
}

function openExistingSeriesView(title, synopsis, status) {
    currentActiveSeriesTitle = title;
    document.getElementById('view-seri-title').textContent = title;
    document.getElementById('view-seri-synopsis').textContent = synopsis;
    const badge = document.getElementById('view-seri-status-badge');
    badge.textContent = status;

    const activeUser = localStorage.getItem('ea_plan_active_user') || "default_user";
    const seriesKey = 'user_series_detailed_' + activeUser;
    let seriesList = JSON.parse(localStorage.getItem(seriesKey)) || [];
    const found = seriesList.find(s => s.title === title);

    if (found) {
        renderSeriesWorksContainer(found.works);
    } else {
        renderSeriesWorksContainer([]);
    }

    document.getElementById('tab-content-creator-view').style.display = 'none';
    document.getElementById('tab-content-view-detail-seri').style.display = 'flex';
}

/* --- FORMAT TEKS SINOPSIS SERI --- */
function applySeriTextFormat(tag) {
    const textarea = document.getElementById('seri-sinopsis-textarea');
    const start = textarea.selectionStart;
    const end = textarea.selectionEnd;
    const selectedText = textarea.value.substring(start, end);

    let formatted = selectedText;
    if (tag === 'h1') formatted = `# ${selectedText}`;
    else if (tag === 'h2') formatted = `## ${selectedText}`;
    else if (tag === 'h3') formatted = `### ${selectedText}`;

    textarea.value = textarea.value.substring(0, start) + formatted + textarea.value.substring(end);
}

function applySeriInline(type) {
    const textarea = document.getElementById('seri-sinopsis-textarea');
    const start = textarea.selectionStart;
    const end = textarea.selectionEnd;
    const selectedText = textarea.value.substring(start, end);

    let formatted = selectedText;
    if (type === 'bold') formatted = `**${selectedText}**`;
    else if (type === 'italic') formatted = `*${selectedText}*`;

    textarea.value = textarea.value.substring(0, start) + formatted + textarea.value.substring(end);
}

function insertSeriLink() {
    const url = prompt("Masukkan URL Tautan:", "https://");
    if (!url) return;
    const textarea = document.getElementById('seri-sinopsis-textarea');
    const start = textarea.selectionStart;
    const end = textarea.selectionEnd;
    const selectedText = textarea.value.substring(start, end) || "Tautan";

    const formatted = `[${selectedText}](${url})`;
    textarea.value = textarea.value.substring(0, start) + formatted + textarea.value.substring(end);
}

function openDetailKarya(kategoriName) {
    document.getElementById('dk-category-select').value = kategoriName;
    document.getElementById('tab-content-kategori-karya').style.display = 'none';
    document.getElementById('tab-content-detail-karya').style.display = 'flex';
    loadSeriesOptions();
}

function changeCategorySelection(newCategory) {
    console.log("Kategori karya diubah menjadi: " + newCategory);
}

function backToKategoriKarya() {
    document.getElementById('tab-content-detail-karya').style.display = 'none';
    document.getElementById('tab-content-kategori-karya').style.display = 'flex';
}

function openIsiKarya() {
    const selectedKat = document.getElementById('dk-category-select').value;
    document.getElementById('isi-karya-label-title').textContent = "Kategori " + selectedKat;

    document.getElementById('tab-content-detail-karya').style.display = 'none';
    document.getElementById('tab-content-isi-karya').style.display = 'flex';
}

function backToDetailKarya() {
    document.getElementById('tab-content-isi-karya').style.display = 'none';
    document.getElementById('tab-content-detail-karya').style.display = 'flex';
}

function openReviewKarya() {
    const category = document.getElementById('dk-category-select').value;
    const title = document.getElementById('karya-judul-input').value.trim() || "Tanpa Judul";
    const desc = document.getElementById('karya-deskripsi-input').value.trim() || "Belum ada deskripsi.";
    const genre = document.getElementById('karya-genre-select').value || "Belum Dipilih";
    const series = document.getElementById('karya-series-select').value || "Tanpa Seri";
    const content = document.getElementById('isi-karya-textarea').value.trim() || "Belum ada isi cerita.";

    document.getElementById('rev-category-label').textContent = category;
    document.getElementById('rev-title-display').textContent = title;
    document.getElementById('rev-genre-display').textContent = "Genre: " + genre;
    document.getElementById('rev-series-display').textContent = "Seri: " + series;
    document.getElementById('rev-desc-display').textContent = desc;
    document.getElementById('rev-content-display').textContent = content;

    const coverBox = document.getElementById('review-cover-box');
    if (temporaryCoverBase64) {
        coverBox.innerHTML = `<img src="${temporaryCoverBase64}" alt="Cover" style="width:100%; height:100%; object-fit:cover;">`;
    } else {
        coverBox.innerHTML = `<span class="material-icons" style="font-size: 36px; color: #aaa;">image</span><span style="font-size: 10px; color: #777;">Cover</span>`;
    }

    document.getElementById('tab-content-isi-karya').style.display = 'none';
    document.getElementById('tab-content-review-karya').style.display = 'flex';
}

function backToIsiKarya() {
    document.getElementById('tab-content-review-karya').style.display = 'none';
    document.getElementById('tab-content-isi-karya').style.display = 'flex';
}

function publishKaryaFinal() {
    alert("Kategori karya berhasil diterbitkan dan dipublikasikan!");
    document.getElementById('tab-content-review-karya').style.display = 'none';
    document.getElementById('tab-content-home').style.display = 'flex';
}

/* --- FITUR FORMAT TEKS DI HALAMAN ISI KARYA --- */
function applyIsiTextFormat(tag) {
    const textarea = document.getElementById('isi-karya-textarea');
    const start = textarea.selectionStart;
    const end = textarea.selectionEnd;
    const selectedText = textarea.value.substring(start, end);

    let formatted = selectedText;
    if (tag === 'Heading 1') formatted = `# ${selectedText}`;
    else if (tag === 'Heading 2') formatted = `## ${selectedText}`;
    else if (tag === 'Heading 3') formatted = `### ${selectedText}`;

    textarea.value = textarea.value.substring(0, start) + formatted + textarea.value.substring(end);
}

function applyIsiInline(type) {
    const textarea = document.getElementById('isi-karya-textarea');
    const start = textarea.selectionStart;
    const end = textarea.selectionEnd;
    const selectedText = textarea.value.substring(start, end);

    let formatted = selectedText;
    if (type === 'bold') formatted = `**${selectedText}**`;
    else if (type === 'italic') formatted = `*${selectedText}*`;

    textarea.value = textarea.value.substring(0, start) + formatted + textarea.value.substring(end);
}

function insertIsiLink() {
    const url = prompt("Masukkan URL Tautan:", "https://");
    if (!url) return;
    const textarea = document.getElementById('isi-karya-textarea');
    const start = textarea.selectionStart;
    const end = textarea.selectionEnd;
    const selectedText = textarea.value.substring(start, end) || "Tautan";

    const formatted = `[${selectedText}](${url})`;
    textarea.value = textarea.value.substring(0, start) + formatted + textarea.value.substring(end);
}

function loadSeriesOptions() {
    const select = document.getElementById('karya-series-select');
    if (!select) return;

    const activeUser = localStorage.getItem('ea_plan_active_user') || "default_user";
    const seriesKey = 'user_series_' + activeUser;
    let seriesList = JSON.parse(localStorage.getItem(seriesKey)) || ['Kekasih Pertama & Terakhir', 'Zona Nyaman'];

    select.innerHTML = '<option value="" disabled selected>Pilih Seri Kategori</option>';
    seriesList.forEach(seri => {
        const opt = document.createElement('option');
        opt.value = seri;
        opt.textContent = seri;
        select.appendChild(opt);
    });
}

function applyTextFormat(tag) {
    const textarea = document.getElementById('karya-deskripsi-input');
    const start = textarea.selectionStart;
    const end = textarea.selectionEnd;
    const selectedText = textarea.value.substring(start, end);

    let formatted = selectedText;
    if (tag === 'h1') formatted = `# ${selectedText}`;
    else if (tag === 'h2') formatted = `## ${selectedText}`;
    else if (tag === 'h3') formatted = `### ${selectedText}`;

    textarea.value = textarea.value.substring(0, start) + formatted + textarea.value.substring(end);
}

function applyInlineFormat(type) {
    const textarea = document.getElementById('karya-deskripsi-input');
    const start = textarea.selectionStart;
    const end = textarea.selectionEnd;
    const selectedText = textarea.value.substring(start, end);

    let formatted = selectedText;
    if (type === 'bold') formatted = `**${selectedText}**`;
    else if (type === 'italic') formatted = `*${selectedText}*`;
    else if (type === 'plain') formatted = selectedText;

    textarea.value = textarea.value.substring(0, start) + formatted + textarea.value.substring(end);
}

function insertLinkPrompt() {
    const url = prompt("Masukkan URL Tautan:", "https://");
    if (!url) return;
    const textarea = document.getElementById('karya-deskripsi-input');
    const start = textarea.selectionStart;
    const end = textarea.selectionEnd;
    const selectedText = textarea.value.substring(start, end) || "Tautan";

    const formatted = `[${selectedText}](${url})`;
    textarea.value = textarea.value.substring(0, start) + formatted + textarea.value.substring(end);
}

function openMyJournalSection() {
    document.getElementById('tab-content-journal').style.display = 'none';
    document.getElementById('tab-content-my-journal').style.display = 'flex';
    loadUserJournals('Semua');
}

function switchToCreatorProfile() {
    document.getElementById('tab-content-profile').style.display = 'none';
    document.getElementById('tab-content-creator-view').style.display = 'flex';
    renderCreatorSeries();
}

function backToProfileMenu() {
    document.getElementById('tab-content-creator-view').style.display = 'none';
    document.getElementById('tab-content-profile').style.display = 'flex';
}

function switchCreatorTab(event, tabName) {
    const tabs = document.querySelectorAll('.creator-tab-item');
    tabs.forEach(tab => tab.classList.remove('active'));
    event.currentTarget.classList.add('active');

    document.getElementById('creator-tab-seri').style.display = 'none';
    document.getElementById('creator-tab-karya').style.display = 'none';
    document.getElementById('creator-tab-komunitas').style.display = 'none';
    document.getElementById('creator-tab-journey').style.display = 'none';
    document.getElementById('creator-tab-info').style.display = 'none';

    if (tabName === 'seri') document.getElementById('creator-tab-seri').style.display = 'block';
    else if (tabName === 'karya') document.getElementById('creator-tab-karya').style.display = 'block';
    else if (tabName === 'komunitas') document.getElementById('creator-tab-komunitas').style.display = 'block';
    else if (tabName === 'journey') document.getElementById('creator-tab-journey').style.display = 'block';
    else if (tabName === 'info') document.getElementById('creator-tab-info').style.display = 'block';
}

/* --- MANAJEMEN JURNAL SAYA --- */
let currentJournalFilter = 'Semua';

function openJournalEditor(editId = null) {
    document.getElementById('my-journal-edit-id').value = '';
    document.getElementById('my-journal-title').value = '';
    document.getElementById('my-journal-text').value = '';
    document.getElementById('my-journal-category').value = 'Ide';
    document.getElementById('modal-editor-title-text').textContent = 'Buat Catatan / Jurnal Baru';
    
    if (editId) {
        const activeUser = localStorage.getItem('ea_plan_active_user') || "default_user";
        const storageKey = 'user_journals_' + activeUser;
        let journals = JSON.parse(localStorage.getItem(storageKey)) || [];
        const target = journals.find(item => item.id === editId);

        if (target) {
            document.getElementById('my-journal-edit-id').value = target.id;
            document.getElementById('my-journal-title').value = target.title;
            document.getElementById('my-journal-category').value = target.category;
            document.getElementById('my-journal-text').value = target.content;
            document.getElementById('modal-editor-title-text').textContent = 'Edit Catatan / Jurnal';
        }
    }

    updateWordAndCharCount();
    document.getElementById('journal-editor-modal').style.display = 'flex';
}

function closeJournalEditor() {
    document.getElementById('journal-editor-modal').style.display = 'none';
}

function updateWordAndCharCount() {
    const text = document.getElementById('my-journal-text').value.trim();
    const charCount = text.length;
    const wordCount = text === '' ? 0 : text.split(/\s+/).length;
    document.getElementById('word-char-counter').textContent = `${wordCount} Kata | ${charCount} Karakter`;
}

function saveUserJournalEntry() {
    const editId = document.getElementById('my-journal-edit-id').value;
    const title = document.getElementById('my-journal-title').value.trim();
    const category = document.getElementById('my-journal-category').value;
    const content = document.getElementById('my-journal-text').value.trim();
    const activeUser = localStorage.getItem('ea_plan_active_user') || "default_user";

    if (!title || !content) {
        alert("Judul dan isi catatan tidak boleh kosong!");
        return;
    }

    const storageKey = 'user_journals_' + activeUser;
    let journals = JSON.parse(localStorage.getItem(storageKey)) || [];

    if (editId) {
        journals = journals.map(item => {
            if (item.id == editId) {
                return { ...item, title, category, content, date: item.date + ' (Diperbarui)' };
            }
            return item;
        });
        alert("Catatan jurnal berhasil diperbarui!");
    } else {
        const newEntry = {
            id: Date.now(),
            title: title,
            category: category,
            content: content,
            date: new Date().toLocaleDateString('id-ID', { day: 'numeric', month: 'short', year: 'numeric' })
        };
        journals.unshift(newEntry);
        alert("Catatan jurnal berhasil disimpan!");
    }

    localStorage.setItem(storageKey, JSON.stringify(journals));
    closeJournalEditor();
    loadUserJournals(currentJournalFilter);
}

function loadUserJournals(filter = 'Semua', searchQuery = '') {
    const container = document.getElementById('my-journal-cards-container');
    if (!container) return;

    const activeUser = localStorage.getItem('ea_plan_active_user') || "default_user";
    const storageKey = 'user_journals_' + activeUser;
    let journals = JSON.parse(localStorage.getItem(storageKey)) || [];

    if (filter !== 'Semua') {
        journals = journals.filter(item => item.category === filter);
    }

    if (searchQuery.trim() !== '') {
        const query = searchQuery.toLowerCase();
        journals = journals.filter(item => 
            item.title.toLowerCase().includes(query) || 
            item.content.toLowerCase().includes(query)
        );
    }

    container.innerHTML = '';

    if (journals.length === 0) {
        container.innerHTML = '<p class="empty-tab-text">Belum ada catatan yang ditemukan.</p>';
        return;
    }

    journals.forEach(item => {
        const card = document.createElement('div');
        card.className = 'user-journal-card';
        card.innerHTML = `
            <div class="ujc-header">
                <span class="ujc-title">${escapeHtml(item.title)}</span>
                <span class="ujc-badge">${escapeHtml(item.category)}</span>
            </div>
            <p class="ujc-body">${escapeHtml(item.content)}</p>
            <div class="ujc-footer">
                <span>${item.date}</span>
                <div class="ujc-actions">
                    <button class="ujc-edit" onclick="openJournalEditor(${item.id})">Edit</button>
                    <button class="ujc-delete" onclick="deleteUserJournal(${item.id})">Hapus</button>
                </div>
            </div>
        `;
        container.appendChild(card);
    });
}

function filterJournals(category, event) {
    currentJournalFilter = category;
    const chips = document.querySelectorAll('.filter-chip');
    chips.forEach(chip => chip.classList.remove('active'));
    event.currentTarget.classList.add('active');

    const searchQuery = document.getElementById('journal-search-input').value;
    loadUserJournals(category, searchQuery);
}

function handleJournalSearch() {
    const searchQuery = document.getElementById('journal-search-input').value;
    loadUserJournals(currentJournalFilter, searchQuery);
}

function deleteUserJournal(id) {
    if (!confirm("Hapus catatan jurnal ini?")) return;

    const activeUser = localStorage.getItem('ea_plan_active_user') || "default_user";
    const storageKey = 'user_journals_' + activeUser;
    let journals = JSON.parse(localStorage.getItem(storageKey)) || [];

    journals = journals.filter(item => item.id !== id);
    localStorage.setItem(storageKey, JSON.stringify(journals));

    const searchQuery = document.getElementById('journal-search-input').value;
    loadUserJournals(currentJournalFilter, searchQuery);
}

function escapeHtml(text) {
    const map = {
        '&': '&amp;',
        '<': '&lt;',
        '>': '&gt;',
        '"': '&quot;',
        "'": '&#039;'
    };
    return text.replace(/[&<>"']/g, function(m) { return map[m]; });
}

let verifiedResetEmail = "";
function openForgotPassword(event) {
    event.preventDefault();
    document.getElementById('forgot-modal').style.display = 'flex';
    document.getElementById('reset-email').value = '';
    document.getElementById('reset-new-password').value = '';
    document.getElementById('new-pass-group').style.display = 'none';
    document.getElementById('modal-error').style.display = 'none';
    document.getElementById('modal-action-btn').textContent = "Cek Email";
    verifiedResetEmail = "";
}

function closeForgotPassword() {
    document.getElementById('forgot-modal').style.display = 'none';
}

function processForgotPassword() {
    const emailInput = document.getElementById('reset-email').value.trim().toLowerCase();
    const modalError = document.getElementById('modal-error');
    const newPassGroup = document.getElementById('new-pass-group');
    const actionBtn = document.getElementById('modal-action-btn');

    if (!verifiedResetEmail) {
        const registeredName = localStorage.getItem('user_' + emailInput);
        if (!registeredName) {
            modalError.textContent = "Email tidak ditemukan di sistem!";
            modalError.style.display = 'block';
            return;
        }
        verifiedResetEmail = emailInput;
        modalError.style.display = 'none';
        newPassGroup.style.display = 'flex';
        actionBtn.textContent = "Simpan Password Baru";
        document.getElementById('reset-email').disabled = true;
    } else {
        const newPassword = document.getElementById('reset-new-password').value;
        const isValidPass = /^[A-Z].*(?=\d)(?=.*[\W_]).*$/.test(newPassword);
        if (!isValidPass) {
            modalError.textContent = "Password baru harus diawali huruf kapital, mengandung angka, dan simbol!";
            modalError.style.display = 'block';
            return;
        }
        localStorage.setItem('pass_' + verifiedResetEmail, newPassword);
        alert("Password berhasil diubah!");
        closeForgotPassword();
    }
}

function showError(message) {
    const errorBox = document.getElementById('error-message');
    errorBox.textContent = message;
    errorBox.style.display = 'block';
}

function transitionToDashboard() {
    const authScreen = document.getElementById('auth-screen');
    const homeScreen = document.getElementById('home-screen');
    authScreen.style.opacity = '0';
    setTimeout(() => {
        authScreen.style.display = 'none';
        homeScreen.style.display = 'flex';
    }, 400);
}

function logoutAccount() {
    localStorage.removeItem('ea_plan_active_user');
    location.reload();
}

function switchTab(type) {
    const tabLogin = document.getElementById('tab-login');
    const tabSignup = document.getElementById('tab-signup');
    const formLogin = document.getElementById('form-login');
    const formSignup = document.getElementById('form-signup');
    const errorBox = document.getElementById('error-message');
    errorBox.style.display = 'none';

    if (type === 'login') {
        tabLogin.classList.add('active');
        tabSignup.classList.remove('active');
        formLogin.style.display = 'flex';
        formSignup.style.display = 'none';
    } else {
        tabSignup.classList.add('active');
        tabLogin.classList.remove('active');
        formSignup.style.display = 'flex';
        formLogin.style.display = 'none';
    }
}
