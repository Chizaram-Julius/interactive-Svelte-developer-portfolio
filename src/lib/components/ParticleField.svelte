<script>
  import { onMount } from 'svelte';

  let canvas;
  let frame;
  let reduceMotion = false;

  onMount(() => {
    const ctx = canvas.getContext('2d');
    const media = window.matchMedia('(prefers-reduced-motion: reduce)');
    reduceMotion = media.matches;
    let particles = [];
    let pointer = { x: 0.5, y: 0.5 };

    const resize = () => {
      const ratio = Math.min(window.devicePixelRatio || 1, 2);
      canvas.width = Math.floor(canvas.offsetWidth * ratio);
      canvas.height = Math.floor(canvas.offsetHeight * ratio);
      ctx.setTransform(ratio, 0, 0, ratio, 0, 0);
      const count = Math.min(90, Math.max(34, Math.floor(canvas.offsetWidth / 16)));
      particles = Array.from({ length: count }, (_, index) => ({
        x: Math.random() * canvas.offsetWidth,
        y: Math.random() * canvas.offsetHeight,
        vx: (Math.random() - 0.5) * 0.35,
        vy: (Math.random() - 0.5) * 0.35,
        size: 1 + Math.random() * 2.6,
        phase: index * 0.13
      }));
    };

    const move = (event) => {
      pointer = {
        x: event.clientX / Math.max(window.innerWidth, 1),
        y: event.clientY / Math.max(window.innerHeight, 1)
      };
    };

    const draw = (time = 0) => {
      const width = canvas.offsetWidth;
      const height = canvas.offsetHeight;
      ctx.clearRect(0, 0, width, height);
      const gradient = ctx.createRadialGradient(
        pointer.x * width,
        pointer.y * height,
        20,
        width * 0.5,
        height * 0.5,
        Math.max(width, height)
      );
      gradient.addColorStop(0, 'rgba(124, 247, 212, 0.26)');
      gradient.addColorStop(0.42, 'rgba(155, 188, 255, 0.14)');
      gradient.addColorStop(1, 'rgba(9, 11, 16, 0)');
      ctx.fillStyle = gradient;
      ctx.fillRect(0, 0, width, height);

      for (const point of particles) {
        if (!reduceMotion) {
          point.x += point.vx;
          point.y += point.vy;
          if (point.x < -10) point.x = width + 10;
          if (point.x > width + 10) point.x = -10;
          if (point.y < -10) point.y = height + 10;
          if (point.y > height + 10) point.y = -10;
        }
        const pulse = reduceMotion ? 0.7 : 0.45 + Math.sin(time * 0.001 + point.phase) * 0.25;
        ctx.beginPath();
        ctx.fillStyle = `rgba(240, 247, 255, ${0.28 + pulse * 0.42})`;
        ctx.arc(point.x, point.y, point.size, 0, Math.PI * 2);
        ctx.fill();
      }

      ctx.strokeStyle = 'rgba(124, 247, 212, 0.13)';
      ctx.lineWidth = 1;
      for (let i = 0; i < particles.length; i += 1) {
        for (let j = i + 1; j < particles.length; j += 1) {
          const a = particles[i];
          const b = particles[j];
          const distance = Math.hypot(a.x - b.x, a.y - b.y);
          if (distance < 112) {
            ctx.globalAlpha = 1 - distance / 112;
            ctx.beginPath();
            ctx.moveTo(a.x, a.y);
            ctx.lineTo(b.x, b.y);
            ctx.stroke();
          }
        }
      }
      ctx.globalAlpha = 1;
      frame = requestAnimationFrame(draw);
    };

    resize();
    draw();
    window.addEventListener('resize', resize);
    window.addEventListener('pointermove', move);
    return () => {
      cancelAnimationFrame(frame);
      window.removeEventListener('resize', resize);
      window.removeEventListener('pointermove', move);
    };
  });
</script>

<canvas bind:this={canvas} aria-hidden="true"></canvas>

<style>
  canvas {
    position: absolute;
    inset: 0;
    width: 100%;
    height: 100%;
  }
</style>
