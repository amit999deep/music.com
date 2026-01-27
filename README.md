[Uploading java.js…]()
const audioPlayer = document.getElementById('audio-player');
const masterPlay = document.getElementById('master-play');
const progressBar = document.getElementById('progress-bar');
const currentTitle = document.getElementById('current-title');
const currentArtist = document.getElementById('current-artist');
const currentArt = document.querySelector('.current-art');
const currentTimeSpan = document.getElementById('current-time');
const totalDurationSpan = document.getElementById('total-duration');

// Song Data
const songs = [
    {
        title: "Don't Even Text",
        artist: "Tsumyoki x Gini",
        path: "Tsumyoki_X_Gini don t Even Text.mp4",
        coverPath: "cover1.png"
    },
    {
        title: "See You Again",
        artist: "Wiz Khalifa",
        path: "Wiz_Khalifa_-_See_You_Again_.webm",
        coverPath: "cover2.png"
    },
    {
        title: "Sapphire",
        artist: "Ed Sheeran",
        path: "Ed_Sheeran_-_Sapphire_.webm",
        coverPath: "cover3.png"
    },
    {
        title: "Perfect",
        artist: "Ed Sheeran",
        path: "Ed_sheeran_.mp4",
        coverPath: "cover4.png"
    }
];

let songIndex = 0;
let isPlaying = false;

// Load initial song
loadSong(songs[songIndex]);

function loadSong(song) {
    currentTitle.textContent = song.title;
    currentArtist.textContent = song.artist;
    audioPlayer.src = song.path;

    // Update current art
    currentArt.style.backgroundImage = `url('${song.coverPath}')`;
}

function togglePlay() {
    if (isPlaying) {
        pauseSong();
    } else {
        resumeSong();
    }
}

function resumeSong() {
    audioPlayer.play();
    isPlaying = true;
    masterPlay.classList.remove('fa-play-circle');
    masterPlay.classList.add('fa-pause-circle');
}

function pauseSong() {
    audioPlayer.pause();
    isPlaying = false;
    masterPlay.classList.add('fa-play-circle');
    masterPlay.classList.remove('fa-pause-circle');
}

// Play specific song from card
function playSong(index) {
    songIndex = index;
    loadSong(songs[songIndex]);
    resumeSong();
}

function prevSong() {
    songIndex--;
    if (songIndex < 0) {
        songIndex = songs.length - 1;
    }
    loadSong(songs[songIndex]);
    if (isPlaying) resumeSong();
}

function nextSong() {
    songIndex++;
    if (songIndex > songs.length - 1) {
        songIndex = 0;
    }
    loadSong(songs[songIndex]);
    if (isPlaying) resumeSong();
}

// Update Progress Bar
audioPlayer.addEventListener('timeupdate', (e) => {
    const { duration, currentTime } = e.srcElement;
    if (isNaN(duration)) return;

    const progressPercent = (currentTime / duration) * 100;
    progressBar.value = progressPercent;

    // Time formatting
    const durationMinutes = Math.floor(duration / 60);
    let durationSeconds = Math.floor(duration % 60);
    if (durationSeconds < 10) durationSeconds = `0${durationSeconds}`;

    // Delay setting duration to avoid NaN popping up
    if (durationSeconds) {
        totalDurationSpan.textContent = `${durationMinutes}:${durationSeconds}`;
    }

    const currentMinutes = Math.floor(currentTime / 60);
    let currentSeconds = Math.floor(currentTime % 60);
    if (currentSeconds < 10) currentSeconds = `0${currentSeconds}`;
    currentTimeSpan.textContent = `${currentMinutes}:${currentSeconds}`;
});

// Set Progress
progressBar.addEventListener('click', setProgress); // Click anywhere on bar
progressBar.addEventListener('change', setProgress); // Drag

function setProgress(e) {
    // Determine click position
    const width = this.clientWidth;
    // For input type range, value is directly accessible
    // But if we want precise click seeking:
    // const clickX = e.offsetX;
    // const duration = audioPlayer.duration;
    // audioPlayer.currentTime = (clickX / width) * duration;

    // Using value from range input:
    const duration = audioPlayer.duration;
    audioPlayer.currentTime = (progressBar.value / 100) * duration;
}


// Auto next song
audioPlayer.addEventListener('ended', nextSong);

window.onload = function () {
    alert("Completing soon");
};

function showAlert() {
    alert("creat in progress by deep");
}
