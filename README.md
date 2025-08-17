<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flashcards - Aprenda de forma divertida</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        .flashcard-container {
            perspective: 1000px;
            width: 100%;
            max-width: 500px;
            height: 300px;
            margin: 0 auto;
        }
        
        .flashcard {
            width: 100%;
            height: 100%;
            position: relative;
            transform-style: preserve-3d;
            transition: transform 0.6s;
            cursor: pointer;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            border-radius: 15px;
        }
        
        .flashcard.flipped {
            transform: rotateY(180deg);
        }
        
        .flashcard-front, .flashcard-back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 20px;
            border-radius: 15px;
            box-sizing: border-box;
        }
        
        .flashcard-back {
            transform: rotateY(180deg);
        }
        
        .category-badge {
            position: absolute;
            top: 15px;
            right: 15px;
            padding: 5px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease-out forwards;
        }
        
        .share-btn {
            transition: all 0.3s;
        }
        
        .share-btn:hover {
            transform: scale(1.1);
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen font-sans">
    <div class="container mx-auto px-4 py-8">
        <!-- Header -->
        <header class="text-center mb-12 fade-in">
            <h1 class="text-4xl font-bold text-indigo-700 mb-2">Flashcards</h1>
            <p class="text-gray-600">Aprenda de forma divertida e eficiente</p>
            <div class="w-24 h-1 bg-indigo-500 mx-auto mt-4 rounded-full"></div>
        </header>
        
        <!-- Main Content -->
        <main>
            <!-- Flashcard Display -->
            <div class="mb-12 fade-in" style="animation-delay: 0.2s;">
                <div class="flashcard-container mb-8">
                    <div class="flashcard bg-white" id="flashcard">
                        <div class="flashcard-front flex-col">
                            <div class="category-badge bg-indigo-100 text-indigo-800" id="category-badge">Programação</div>
                            <div class="text-center px-6">
                                <i class="fas fa-question-circle text-5xl text-indigo-400 mb-6"></i>
                                <h2 class="text-2xl font-semibold text-gray-800 mb-4" id="question">O que é Python?</h2>
                                <p class="text-gray-500">Clique para ver a resposta</p>
                            </div>
                        </div>
                        <div class="flashcard-back bg-indigo-50">
                            <div class="text-center px-6">
                                <i class="fas fa-lightbulb text-5xl text-yellow-400 mb-6"></i>
                                <h2 class="text-2xl font-semibold text-gray-800 mb-4">Resposta</h2>
                                <p class="text-gray-700 text-lg" id="answer">Python é uma linguagem de programação</p>
                            </div>
                        </div>
                    </div>
                </div>
                
                <div class="flex justify-center space-x-4">
                    <button id="prev-btn" class="px-6 py-2 bg-gray-200 text-gray-700 rounded-full hover:bg-gray-300 transition">
                        <i class="fas fa-arrow-left mr-2"></i>Anterior
                    </button>
                    <button id="next-btn" class="px-6 py-2 bg-indigo-600 text-white rounded-full hover:bg-indigo-700 transition">
                        Próximo<i class="fas fa-arrow-right ml-2"></i>
                    </button>
                </div>
            </div>
            
            <!-- Add New Flashcard -->
            <div class="bg-white rounded-lg shadow-md p-6 mb-8 fade-in" style="animation-delay: 0.4s;">
                <h2 class="text-xl font-semibold text-gray-800 mb-4">Adicionar Novo Flashcard</h2>
                <form id="add-flashcard-form" class="space-y-4">
                    <div>
                        <label for="category" class="block text-sm font-medium text-gray-700 mb-1">Categoria</label>
                        <select id="category" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-indigo-500 focus:border-indigo-500">
                            <option value="Programação">Programação</option>
                            <option value="Geografia">Geografia</option>
                            <option value="História">História</option>
                            <option value="Matemática">Matemática</option>
                            <option value="Ciências">Ciências</option>
                            <option value="Inglês">Inglês</option>
                        </select>
                    </div>
                    <div>
                        <label for="new-question" class="block text-sm font-medium text-gray-700 mb-1">Pergunta</label>
                        <input type="text" id="new-question" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-indigo-500 focus:border-indigo-500" placeholder="Digite a pergunta">
                    </div>
                    <div>
                        <label for="new-answer" class="block text-sm font-medium text-gray-700 mb-1">Resposta</label>
                        <textarea id="new-answer" rows="3" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-indigo-500 focus:border-indigo-500" placeholder="Digite a resposta"></textarea>
                    </div>
                    <button type="submit" class="w-full bg-green-600 text-white py-2 px-4 rounded-lg hover:bg-green-700 transition flex items-center justify-center">
                        <i class="fas fa-plus-circle mr-2"></i> Adicionar Flashcard
                    </button>
                </form>
            </div>
            
            <!-- Share Section -->
            <div class="bg-white rounded-lg shadow-md p-6 fade-in" style="animation-delay: 0.6s;">
                <h2 class="text-xl font-semibold text-gray-800 mb-4">Compartilhar Flashcard</h2>
                <div class="flex flex-col sm:flex-row items-center space-y-4 sm:space-y-0 sm:space-x-4">
                    <input type="text" id="share-link" class="flex-grow px-4 py-2 border border-gray-300 rounded-lg focus:ring-indigo-500 focus:border-indigo-500" readonly>
                    <button id="copy-btn" class="px-6 py-2 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition flex items-center justify-center whitespace-nowrap">
                        <i class="fas fa-copy mr-2"></i> Copiar Link
                    </button>
                    <div class="flex space-x-2">
                        <button class="share-btn p-2 bg-blue-500 text-white rounded-full">
                            <i class="fab fa-facebook-f"></i>
                        </button>
                        <button class="share-btn p-2 bg-blue-400 text-white rounded-full">
                            <i class="fab fa-twitter"></i>
                        </button>
                        <button class="share-btn p-2 bg-red-500 text-white rounded-full">
                            <i class="fab fa-whatsapp"></i>
                        </button>
                    </div>
                </div>
            </div>
        </main>
        
        <!-- Footer -->
        <footer class="mt-16 text-center text-gray-500 text-sm fade-in" style="animation-delay: 0.8s;">
        </footer>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Sample flashcards data
            const flashcards = [
                {
                    category: "Programação",
                    question: "O que é Python?",
                    answer: "Python é uma linguagem de programação de alto nível, interpretada, de script, imperativa, orientada a objetos, funcional, de tipagem dinâmica e forte."
                },
                {
                    category: "Geografia",
                    question: "Qual a capital da França?",
                    answer: "A capital da França é Paris."
                },
                {
                    category: "História",
                    question: "Quando ocorreu a Segunda Guerra Mundial?",
                    answer: "A Segunda Guerra Mundial ocorreu entre 1939 e 1945."
                },
                {
                    category: "Matemática",
                    question: "Quanto é π (pi) aproximadamente?",
                    answer: "π é aproximadamente 3.14159."
                }
            ];
            
            let currentCardIndex = 0;
            const flashcardElement = document.getElementById('flashcard');
            const questionElement = document.getElementById('question');
            const answerElement = document.getElementById('answer');
            const categoryBadge = document.getElementById('category-badge');
            const prevBtn = document.getElementById('prev-btn');
            const nextBtn = document.getElementById('next-btn');
            const addFlashcardForm = document.getElementById('add-flashcard-form');
            const shareLink = document.getElementById('share-link');
            const copyBtn = document.getElementById('copy-btn');
            
            // Initialize the first flashcard
            updateFlashcard();
            
            // Flip animation
            flashcardElement.addEventListener('click', function() {
                flashcardElement.classList.toggle('flipped');
            });
            
            // Navigation buttons
            prevBtn.addEventListener('click', function() {
                if (currentCardIndex > 0) {
                    currentCardIndex--;
                    updateFlashcard();
                    resetCardPosition();
                }
            });
            
            nextBtn.addEventListener('click', function() {
                if (currentCardIndex < flashcards.length - 1) {
                    currentCardIndex++;
                    updateFlashcard();
                    resetCardPosition();
                }
            });
            
            // Add new flashcard
            addFlashcardForm.addEventListener('submit', function(e) {
                e.preventDefault();
                
                const category = document.getElementById('category').value;
                const newQuestion = document.getElementById('new-question').value.trim();
                const newAnswer = document.getElementById('new-answer').value.trim();
                
                if (newQuestion && newAnswer) {
                    flashcards.push({
                        category: category,
                        question: newQuestion,
                        answer: newAnswer
                    });
                    
                    // Clear form
                    addFlashcardForm.reset();
                    
                    // Show success message
                    alert('Flashcard adicionado com sucesso!');
                    
                    // Update to show the new card if it's the first one
                    if (flashcards.length === 1) {
                        currentCardIndex = 0;
                        updateFlashcard();
                    }
                } else {
                    alert('Por favor, preencha tanto a pergunta quanto a resposta.');
                }
            });
            
            // Copy share link
            copyBtn.addEventListener('click', function() {
                generateShareLink();
                shareLink.select();
                document.execCommand('copy');
                
                // Change button text temporarily
                const originalText = copyBtn.innerHTML;
                copyBtn.innerHTML = '<i class="fas fa-check mr-2"></i> Copiado!';
                
                setTimeout(function() {
                    copyBtn.innerHTML = originalText;
                }, 2000);
            });
            
            // Social share buttons
            document.querySelectorAll('.share-btn').forEach(btn => {
                btn.addEventListener('click', function() {
                    generateShareLink();
                    const platform = this.querySelector('i').classList[1].split('-')[1];
                    
                    let shareUrl = '';
                    const text = `Confira este flashcard: "${flashcards[currentCardIndex].question}" - Resposta: "${flashcards[currentCardIndex].answer}"`;
                    
                    switch(platform) {
                        case 'facebook':
                            shareUrl = `https://www.facebook.com/sharer/sharer.php?u=${encodeURIComponent(shareLink.value)}`;
                            break;
                        case 'twitter':
                            shareUrl = `https://twitter.com/intent/tweet?text=${encodeURIComponent(text)}&url=${encodeURIComponent(shareLink.value)}`;
                            break;
                        case 'whatsapp':
                            shareUrl = `https://wa.me/?text=${encodeURIComponent(text + ' ' + shareLink.value)}`;
                            break;
                    }
                    
                    window.open(shareUrl, '_blank', 'width=600,height=400');
                });
            });
            
            // Helper functions
            function updateFlashcard() {
                const currentCard = flashcards[currentCardIndex];
                questionElement.textContent = currentCard.question;
                answerElement.textContent = currentCard.answer;
                categoryBadge.textContent = currentCard.category;
                
                // Update category badge color based on category
                updateCategoryBadgeColor(currentCard.category);
                
                // Update share link
                generateShareLink();
                
                // Update button states
                prevBtn.disabled = currentCardIndex === 0;
                nextBtn.disabled = currentCardIndex === flashcards.length - 1;
            }
            
            function resetCardPosition() {
                if (flashcardElement.classList.contains('flipped')) {
                    flashcardElement.classList.remove('flipped');
                }
            }
            
            function updateCategoryBadgeColor(category) {
                // Remove all color classes
                categoryBadge.className = 'category-badge absolute top-4 right-4 px-3 py-1 rounded-full text-xs font-bold';
                
                // Add specific color based on category
                switch(category) {
                    case 'Programação':
                        categoryBadge.classList.add('bg-indigo-100', 'text-indigo-800');
                        break;
                    case 'Geografia':
                        categoryBadge.classList.add('bg-green-100', 'text-green-800');
                        break;
                    case 'História':
                        categoryBadge.classList.add('bg-yellow-100', 'text-yellow-800');
                        break;
                    case 'Matemática':
                        categoryBadge.classList.add('bg-red-100', 'text-red-800');
                        break;
                    case 'Ciências':
                        categoryBadge.classList.add('bg-blue-100', 'text-blue-800');
                        break;
                    case 'Inglês':
                        categoryBadge.classList.add('bg-purple-100', 'text-purple-800');
                        break;
                    default:
                        categoryBadge.classList.add('bg-gray-100', 'text-gray-800');
                }
            }
            
            function generateShareLink() {
                const currentCard = flashcards[currentCardIndex];
                const baseUrl = window.location.href.split('?')[0];
                const shareText = `Confira este flashcard: ${currentCard.question} - Resposta: ${currentCard.answer}`;
                shareLink.value = `${baseUrl}?share=true&category=${encodeURIComponent(currentCard.category)}&question=${encodeURIComponent(currentCard.question)}&answer=${encodeURIComponent(currentCard.answer)}`;
            }
            
            // Check for shared flashcard in URL
            const urlParams = new URLSearchParams(window.location.search);
            if (urlParams.has('share') && urlParams.has('question') && urlParams.has('answer')) {
                const sharedCategory = urlParams.get('category') || 'Geral';
                const sharedQuestion = decodeURIComponent(urlParams.get('question'));
                const sharedAnswer = decodeURIComponent(urlParams.get('answer'));
                
                // Add to flashcards if not already present
                const isAlreadyAdded = flashcards.some(card => 
                    card.question === sharedQuestion && card.answer === sharedAnswer
                );
                
                if (!isAlreadyAdded) {
                    flashcards.unshift({
                        category: sharedCategory,
                        question: sharedQuestion,
                        answer: sharedAnswer
                    });
                    currentCardIndex = 0;
                    updateFlashcard();
                } else {
                    // Find the index of the shared card
                    const sharedIndex = flashcards.findIndex(card => 
                        card.question === sharedQuestion && card.answer === sharedAnswer
                    );
                    if (sharedIndex !== -1) {
                        currentCardIndex = sharedIndex;
                        updateFlashcard();
                    }
                }
                
                // Clean URL
                history.replaceState(null, '', window.location.pathname);
            }
        });
    </script>
</body>
</html>
