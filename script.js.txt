// ========== СКРИПТ ДЛЯ ГАЛЕРЕИ И ФОРМЫ ==========

document.addEventListener('DOMContentLoaded', function() {
    console.log('✅ Сайт загружен, JavaScript работает');
    
    // ===== 1. СИСТЕМА ЛАЙКОВ =====
    let totalLikes = 0;
    const totalLikesSpan = document.getElementById('total-likes');
    
    function updateTotalLikes() {
        if (totalLikesSpan) {
            totalLikesSpan.textContent = totalLikes;
        }
    }
    
    const likeButtons = document.querySelectorAll('.like-btn');
    
    likeButtons.forEach(button => {
        // Восстанавливаем сохранённые лайки
        const id = button.getAttribute('data-id');
        const savedLikes = localStorage.getItem(`like_${id}`);
        const savedState = localStorage.getItem(`liked_${id}`);
        const likeSpan = button.querySelector('.like-count');
        
        if (savedLikes && likeSpan) {
            likeSpan.textContent = savedLikes;
        }
        
        if (savedState === 'true') {
            button.classList.add('liked');
            button.querySelector('i').classList.remove('far');
            button.querySelector('i').classList.add('fas');
            if (likeSpan) {
                totalLikes += parseInt(likeSpan.textContent) || 0;
            }
        }
        
        button.addEventListener('click', function(e) {
            e.stopPropagation();
            const likesSpan = this.querySelector('.like-count');
            let currentLikes = parseInt(likesSpan.textContent);
            const id = this.getAttribute('data-id');
            
            if (this.classList.contains('liked')) {
                currentLikes--;
                totalLikes--;
                this.classList.remove('liked');
                this.querySelector('i').classList.remove('fas');
                this.querySelector('i').classList.add('far');
                localStorage.setItem(`liked_${id}`, 'false');
            } else {
                currentLikes++;
                totalLikes++;
                this.classList.add('liked');
                this.querySelector('i').classList.remove('far');
                this.querySelector('i').classList.add('fas');
                localStorage.setItem(`liked_${id}`, 'true');
            }
            
            likesSpan.textContent = currentLikes;
            localStorage.setItem(`like_${id}`, currentLikes);
            updateTotalLikes();
            
            // Анимация
            this.style.transform = 'scale(1.2)';
            setTimeout(() => {
                this.style.transform = 'scale(1)';
            }, 200);
        });
    });
    
    updateTotalLikes();
    
    // ===== 2. ФИЛЬТРАЦИЯ ГАЛЕРЕИ =====
    const filterBtns = document.querySelectorAll('.filter-btn');
    const cards = document.querySelectorAll('.image-card');
    const imageCounter = document.getElementById('image-counter');
    
    function updateImageCounter() {
        if
