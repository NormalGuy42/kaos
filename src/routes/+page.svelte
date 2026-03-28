<script lang="ts">
	import { onMount } from 'svelte';

	const DISCORD_URL = 'https://discord.gg/vMt5EThCx';
	const TWITTER_URL = 'https://x.com/kaos';

	const heroText = 'AI Powered Investing,\nBuilt to Grow Your Portfolio';
	const marqueeQuotes =
		'finally a server where people show receipts \u00B7 the AI analysis is better than my broker\u2019s research \u00B7 this is what fintwit should have been \u00B7 joined for the bot, stayed for the community \u00B7 morning briefings are essential \u00B7 my most-used Discord by far \u00B7 no fakers, no cap, just receipts \u00B7 ';

	let charCount = $state(0);
	let showCursor = $state(true);
	let heroDone = $state(false);
	let navVisible = $state(false);

	let rendered = $derived(heroText.slice(0, charCount));

	function reveal(node: HTMLElement) {
		const obs = new IntersectionObserver(
			([e]) => {
				if (e.isIntersecting) {
					node.classList.add('revealed');
					const children = node.querySelectorAll('[data-child]');
					children.forEach((child, i) => {
						setTimeout(() => child.classList.add('revealed'), i * 100);
					});
					const mockups = node.querySelectorAll('[data-mockup]');
					mockups.forEach((mockup, i) => {
						setTimeout(() => mockup.classList.add('revealed'), children.length * 100 + 100 + i * 150);
					});
					obs.unobserve(node);
				}
			},
			{ threshold: 0.08 }
		);
		obs.observe(node);
		return { destroy: () => obs.disconnect() };
	}

	onMount(() => {
		let i = 0;
		const timer = setInterval(() => {
			if (i < heroText.length) {
				charCount = ++i;
			} else {
				clearInterval(timer);
				heroDone = true;
				setTimeout(() => (showCursor = false), 2000);
			}
		}, 30);

		const hero = document.getElementById('hero');
		if (hero) {
			const navObs = new IntersectionObserver(([e]) => (navVisible = !e.isIntersecting), { threshold: 0 });
			navObs.observe(hero);
		}

		return () => clearInterval(timer);
	});
</script>

<svelte:head>
	<title>KAOS — AI Powered Investing</title>
	<meta name="description" content="AI-native investing platform. Research, portfolio analysis, insider tracking, and verified community in one workspace." />
	<meta property="og:title" content="KAOS — AI Powered Investing" />
	<meta property="og:description" content="AI-native investing platform. Research, portfolio analysis, insider tracking, and verified community in one workspace." />
</svelte:head>

<!-- ══════════════════════ 1. NAV ══════════════════════ -->

<nav
	class="fixed top-0 right-0 left-0 z-50 flex h-14 items-center justify-between px-6 transition-all duration-300 md:px-10"
	style="transform: translateY({navVisible ? '0' : '-100%'}); background: rgba(17,17,17,0.85); backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px); border-bottom: 1px solid rgba(255,255,255,0.04);"
>
	<a href="/" class="font-pixel text-[16px] tracking-[0.2em] text-white">KAOS</a>
	<a href={DISCORD_URL} target="_blank" rel="noopener" class="text-[14px] text-body transition-colors hover:text-white">Join Discord &rarr;</a>
</nav>

<!-- ══════════════════════ 2. HERO ══════════════════════ -->

<section id="hero" class="relative px-6 pt-[120px] md:pt-[160px]">
	<div class="mx-auto max-w-[1100px] text-center">
		<p class="font-mono text-[13px] tracking-widest text-[#666666]">AI-native investing platform</p>

		<h1 class="font-pixel hero-gradient mx-auto mt-5 max-w-[900px] text-center text-[clamp(26px,5.5vw,56px)] leading-[1.15]">
			{#each rendered.split('\n') as line, i}
				{#if i > 0}<br />{/if}{line}
			{/each}{#if showCursor}<span class="typewriter-cursor">&#x258C;</span>{/if}
		</h1>

		{#if heroDone}
			<p class="hero-reveal mx-auto mt-7 max-w-[540px] text-center text-[18px] leading-relaxed text-body">
				Research, analysis, insider tracking, and a verified community &mdash; all in one workspace. AI embedded in every layer.
			</p>
			<div class="hero-reveal hero-reveal-d2 mt-7 flex items-center justify-center gap-4">
				<a href={DISCORD_URL} target="_blank" rel="noopener" class="cta-btn">Join the Discord</a>
			</div>
			<p class="hero-reveal hero-reveal-d3 mt-4 font-mono text-[11px] text-dim">Free. No credit card required.</p>
		{/if}
	</div>

	<!-- ── Big Dashboard Mockup ── -->
	<div class="hero-mockup relative mx-auto mt-14 max-w-[1100px]">
		<div class="dashboard-outer">
			<div class="dashboard-titlebar">
				<div class="flex gap-[6px]">
					<span class="h-3 w-3 rounded-full bg-[#3b3b3b]"></span>
					<span class="h-3 w-3 rounded-full bg-[#3b3b3b]"></span>
					<span class="h-3 w-3 rounded-full bg-[#3b3b3b]"></span>
				</div>
				<span class="mx-auto font-mono text-[11px] text-dim">KAOS — Dashboard</span>
				<div class="flex gap-3 text-[11px] text-[#3b3b3b]">
					<span>&#x2500;</span><span>&#x25A1;</span><span>&times;</span>
				</div>
			</div>
			<div class="dashboard-body">
				<!-- ── Sidebar ── -->
				<div class="dashboard-sidebar">
					<div class="mb-5 px-4">
						<span class="font-pixel text-[12px] tracking-[0.15em] text-white">KAOS</span>
					</div>
					<nav class="flex-1 space-y-0.5">
						<div class="nav-item active"><span class="w-4 text-center text-[10px]">&#x25A3;</span> Dashboard</div>
						<div class="nav-item"><span class="w-4 text-center text-[10px]">&#x25CB;</span> Portfolio</div>
						<div class="nav-item"><span class="w-4 text-center text-[10px]">&#x2315;</span> Research</div>
						<div class="nav-item"><span class="w-4 text-center text-[10px]">&#x2606;</span> Watchlist</div>
						<div class="nav-item"><span class="w-4 text-center text-[10px]">&#x25C9;</span> Community</div>
						<div class="nav-item"><span class="w-4 text-center text-[10px]">&#x2691;</span> Insiders</div>
					</nav>
					<div class="border-t border-border px-4 pt-3">
						<div class="flex items-center gap-2">
							<div class="flex h-6 w-6 items-center justify-center rounded-full bg-border text-[9px] text-muted">T</div>
							<span class="font-mono text-[10px] text-dim">trader_0x7F</span>
						</div>
					</div>
				</div>

				<!-- ── Main Content ── -->
				<div class="dashboard-main font-mono">
					<!-- Search bar -->
					<div class="mb-4 flex items-center gap-2 rounded-lg border border-border bg-[#161616] px-3 py-2">
						<span class="text-[11px] text-[#444]">&#x2315;</span>
						<span class="text-[11px] text-dim">Search tickers, companies, people...</span>
					</div>

					<!-- Stock header -->
					<div class="mb-1 flex flex-wrap items-baseline justify-between gap-2">
						<div class="flex items-baseline gap-2">
							<span class="text-[18px] font-bold text-white">NVDA</span>
							<span class="text-[12px] text-dim">NVIDIA Corporation</span>
						</div>
						<div class="flex items-baseline gap-2">
							<span class="text-[18px] font-bold text-white">$892.14</span>
							<span class="text-[12px] text-green">+3.24% today</span>
						</div>
					</div>
					<div class="mb-4 text-[10px] text-[#444]">NASDAQ &middot; Last updated 3:42 PM EST</div>

					<!-- Chart area -->
					<div class="mb-4 rounded-lg border border-border bg-[#161616] p-4">
						<div class="mb-3 flex gap-3 text-[10px]">
							<span class="border-b border-white pb-0.5 text-white">1D</span>
							<span class="text-dim">1W</span>
							<span class="text-dim">1M</span>
							<span class="text-dim">3M</span>
							<span class="text-dim">1Y</span>
							<span class="text-dim">ALL</span>
						</div>
						<div class="flex items-end gap-px" style="height: 100px;">
							<div class="chart-bar" style="height:35%"></div>
							<div class="chart-bar" style="height:38%"></div>
							<div class="chart-bar" style="height:36%"></div>
							<div class="chart-bar" style="height:40%"></div>
							<div class="chart-bar" style="height:42%"></div>
							<div class="chart-bar" style="height:38%"></div>
							<div class="chart-bar" style="height:41%"></div>
							<div class="chart-bar" style="height:45%"></div>
							<div class="chart-bar" style="height:43%"></div>
							<div class="chart-bar" style="height:47%"></div>
							<div class="chart-bar" style="height:50%"></div>
							<div class="chart-bar" style="height:48%"></div>
							<div class="chart-bar" style="height:52%"></div>
							<div class="chart-bar" style="height:49%"></div>
							<div class="chart-bar" style="height:53%"></div>
							<div class="chart-bar" style="height:56%"></div>
							<div class="chart-bar" style="height:52%"></div>
							<div class="chart-bar" style="height:55%"></div>
							<div class="chart-bar" style="height:58%"></div>
							<div class="chart-bar" style="height:54%"></div>
							<div class="chart-bar" style="height:57%"></div>
							<div class="chart-bar" style="height:60%"></div>
							<div class="chart-bar" style="height:63%"></div>
							<div class="chart-bar" style="height:58%"></div>
							<div class="chart-bar" style="height:56%"></div>
							<div class="chart-bar" style="height:60%"></div>
							<div class="chart-bar" style="height:64%"></div>
							<div class="chart-bar" style="height:67%"></div>
							<div class="chart-bar" style="height:62%"></div>
							<div class="chart-bar" style="height:65%"></div>
							<div class="chart-bar" style="height:68%"></div>
							<div class="chart-bar" style="height:71%"></div>
							<div class="chart-bar" style="height:66%"></div>
							<div class="chart-bar" style="height:70%"></div>
							<div class="chart-bar" style="height:73%"></div>
							<div class="chart-bar" style="height:68%"></div>
							<div class="chart-bar" style="height:72%"></div>
							<div class="chart-bar" style="height:75%"></div>
							<div class="chart-bar" style="height:78%"></div>
							<div class="chart-bar" style="height:74%"></div>
							<div class="chart-bar" style="height:77%"></div>
							<div class="chart-bar" style="height:80%"></div>
							<div class="chart-bar" style="height:76%"></div>
							<div class="chart-bar" style="height:79%"></div>
							<div class="chart-bar" style="height:82%"></div>
							<div class="chart-bar" style="height:85%"></div>
							<div class="chart-bar green" style="height:80%"></div>
							<div class="chart-bar green" style="height:84%"></div>
							<div class="chart-bar green" style="height:87%"></div>
							<div class="chart-bar green" style="height:82%"></div>
							<div class="chart-bar green" style="height:86%"></div>
							<div class="chart-bar green" style="height:90%"></div>
							<div class="chart-bar green" style="height:85%"></div>
							<div class="chart-bar green" style="height:88%"></div>
							<div class="chart-bar green" style="height:92%"></div>
						</div>
						<div class="mt-2 flex justify-between text-[9px] text-[#444]">
							<span>9:30 AM</span>
							<span>12:00 PM</span>
							<span>3:42 PM</span>
						</div>
					</div>

					<!-- Stats row -->
					<div class="mb-4 grid grid-cols-2 gap-2 md:grid-cols-4">
						<div class="rounded-lg border border-border bg-[#161616] px-3 py-2.5">
							<div class="text-[9px] text-dim">Mkt Cap</div>
							<div class="mt-0.5 text-[13px] text-white">$2.19T</div>
						</div>
						<div class="rounded-lg border border-border bg-[#161616] px-3 py-2.5">
							<div class="text-[9px] text-dim">P/E Ratio</div>
							<div class="mt-0.5 text-[13px] text-white">72.3</div>
						</div>
						<div class="rounded-lg border border-border bg-[#161616] px-3 py-2.5">
							<div class="text-[9px] text-dim">Volume</div>
							<div class="mt-0.5 text-[13px] text-white">42.1M</div>
						</div>
						<div class="rounded-lg border border-border bg-[#161616] px-3 py-2.5">
							<div class="text-[9px] text-dim">52w High</div>
							<div class="mt-0.5 text-[13px] text-white">$974.00</div>
						</div>
					</div>

					<!-- AI Insight -->
					<div class="rounded-lg border border-border bg-[#161616] p-3.5">
						<div class="mb-2 flex items-center gap-2">
							<span class="rounded bg-border px-1.5 py-0.5 text-[9px] text-muted">&#x26A1; AI</span>
							<span class="text-[10px] text-dim">Insight</span>
						</div>
						<p class="text-[11px] leading-relaxed text-sub">
							Insider cluster buy at current levels mirrors the Nov 2023 setup. 7 of 9 similar patterns saw +12% in 60 days. Call flow confirms directional bias. Earnings in 9 weeks adds volatility &mdash; size accordingly.
						</p>
					</div>
				</div>

				<!-- ── Right Panel ── -->
				<div class="dashboard-panel font-mono">
					<div class="flex-1 border-b border-border p-4">
						<div class="mb-3 flex items-center gap-2">
							<span class="text-[11px] text-dim">AI Copilot</span>
							<span class="rounded bg-border px-1.5 py-0.5 text-[8px] text-muted">BETA</span>
						</div>
						<div class="space-y-2.5">
							<div class="text-right">
								<div class="inline-block max-w-[90%] rounded-lg rounded-br-sm bg-border px-3 py-2 text-left text-[11px] text-[#cccccc]">
									What's the outlook for NVDA?
								</div>
							</div>
							<div>
								<div class="inline-block max-w-[95%] rounded-lg rounded-bl-sm border border-border bg-raised px-3 py-2 text-[11px] text-sub">
									Strong setup. Insider cluster buy aligns with Nov 2023 pattern. Call flow is bullish. Watch earnings in 9 weeks for potential volatility.
								</div>
							</div>
							<div class="text-right">
								<div class="inline-block max-w-[90%] rounded-lg rounded-br-sm bg-border px-3 py-2 text-left text-[11px] text-[#cccccc]">
									What's the risk?
								</div>
							</div>
							<div>
								<div class="inline-block max-w-[95%] rounded-lg rounded-bl-sm border border-border bg-raised px-3 py-2 text-[11px] text-sub">
									Valuation is stretched at 72x. If earnings disappoint, the pullback could be sharp. Concentration risk if NVDA is &gt;25% of portfolio.
								</div>
							</div>
						</div>
						<div class="mt-3 flex items-center rounded-lg border border-border bg-[#161616] px-3 py-2">
							<span class="text-[10px] text-dim">Ask about NVDA...</span>
						</div>
					</div>
					<div class="p-4">
						<div class="mb-3 text-[10px] text-dim">Watchlist</div>
						<div class="space-y-2">
							<div class="flex items-center justify-between text-[11px]">
								<span class="text-white">AAPL</span>
								<div class="flex gap-2">
									<span class="text-muted">$178.42</span>
									<span class="w-12 text-right text-green">+0.41%</span>
								</div>
							</div>
							<div class="flex items-center justify-between text-[11px]">
								<span class="text-white">MSFT</span>
								<div class="flex gap-2">
									<span class="text-muted">$422.18</span>
									<span class="w-12 text-right text-red">-0.22%</span>
								</div>
							</div>
							<div class="flex items-center justify-between text-[11px]">
								<span class="text-white">TSLA</span>
								<div class="flex gap-2">
									<span class="text-muted">$248.91</span>
									<span class="w-12 text-right text-green">+1.83%</span>
								</div>
							</div>
							<div class="flex items-center justify-between text-[11px]">
								<span class="text-white">AMZN</span>
								<div class="flex gap-2">
									<span class="text-muted">$186.34</span>
									<span class="w-12 text-right text-green">+0.92%</span>
								</div>
							</div>
							<div class="flex items-center justify-between text-[11px]">
								<span class="text-white">META</span>
								<div class="flex gap-2">
									<span class="text-muted">$512.67</span>
									<span class="w-12 text-right text-green">+2.14%</span>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
		<!-- Bottom fade -->
		<div class="pointer-events-none absolute right-0 bottom-0 left-0 h-28 bg-linear-to-t from-bg to-transparent"></div>
	</div>
</section>

<!-- ══════════════════════ 3. THE PLATFORM ══════════════════════ -->

<section use:reveal data-reveal class="mx-auto max-w-[1100px] px-6 py-20 md:px-10 md:py-[140px]">
	<p data-child class="font-mono text-[12px] uppercase tracking-[0.2em] text-dim">// THE PLATFORM</p>
	<h2 data-child class="mt-5 max-w-[700px] text-[clamp(28px,4vw,40px)] leading-[1.2] font-normal text-heading">
		Most investing apps give you a chart and wish you luck.
	</h2>
	<div data-child class="mt-8 max-w-[600px] space-y-6">
		<p class="text-[16px] leading-[1.75] text-body">
			KAOS is a complete investing workspace with AI built into every layer. Ask it about any ticker and get an instant read &mdash; price action, insider buying from SEC filings, options flow, earnings context &mdash; in plain English.
		</p>
		<p class="text-[16px] leading-[1.75] text-body">
			Think of it the way Cursor changed coding. Not a chatbot on the side. An intelligence woven into every screen, every decision, every trade.
		</p>
	</div>
</section>

<!-- ══════════════════════ 4. FEATURES ══════════════════════ -->

<section use:reveal data-reveal class="mx-auto max-w-[1100px] px-6 py-20 md:px-10 md:py-[140px]">
	<p data-child class="font-mono text-[12px] uppercase tracking-[0.2em] text-dim">// WHAT&rsquo;S INSIDE</p>
	<h2 data-child class="mt-5 text-[clamp(26px,3.5vw,38px)] leading-[1.2] font-normal text-heading">
		Four tools, one workspace.
	</h2>
	<p data-child class="mt-3 text-[17px] text-muted">Everything that used to require four different apps.</p>

	<div class="features-grid mt-16">
		<!-- ── AI Copilot ── -->
		<div data-child>
			<h3 class="text-[22px] font-medium text-white">AI Copilot</h3>
			<p class="mt-2 max-w-[480px] text-[15px] leading-[1.75] text-muted">
				Ask anything about any ticker. KAOS pulls from SEC filings, price history, insider data, and options flow to give you a real answer.
			</p>
			<div data-mockup class="mini-mockup">
				<div class="mini-mockup-bar">
					<span class="text-[10px] text-dim">AI Copilot</span>
					<div class="flex gap-1.5">
						<span class="mini-dot"></span><span class="mini-dot"></span><span class="mini-dot"></span>
					</div>
				</div>
				<div class="mini-mockup-body space-y-3">
					<div class="text-right">
						<div class="inline-block max-w-[88%] rounded-lg rounded-br-sm bg-[#232323] px-3 py-2 text-left text-[11px] text-[#ccc]">
							What's the bear case for AAPL right now?
						</div>
						<div class="mt-1 text-[9px] text-[#444]">11:42 AM</div>
					</div>
					<div>
						<div class="mb-1 flex items-center gap-1.5">
							<span class="flex h-[18px] w-[18px] items-center justify-center rounded bg-border text-[8px] text-muted">AI</span>
							<span class="text-[9px] text-dim">KAOS AI</span>
						</div>
						<div class="ml-6 max-w-[92%] rounded-lg rounded-tl-sm border border-border bg-[#1c1c1c] px-3 py-2.5 text-[11px] leading-[1.65] text-sub">
							Services revenue growth decelerated to 11.5% YoY &mdash; slowest in 6 quarters. China iPhone sales fell 2.1% while Huawei gained share. AI narrative hasn&rsquo;t translated to hardware upgrades. P/E at 32x prices in growth that hasn&rsquo;t materialized.
							<div class="mt-2 flex gap-2">
								<span class="rounded bg-raised border border-border px-1.5 py-0.5 text-[8px] text-dim">SEC 10-Q</span>
								<span class="rounded bg-raised border border-border px-1.5 py-0.5 text-[8px] text-dim">IDC Report</span>
								<span class="rounded bg-raised border border-border px-1.5 py-0.5 text-[8px] text-dim">Earnings Call</span>
							</div>
						</div>
						<div class="ml-6 mt-1 text-[9px] text-[#444]">11:42 AM &middot; 3 sources</div>
					</div>
					<div class="flex items-center rounded-lg border border-border bg-[#161616] px-3 py-2">
						<span class="text-[10px] text-dim">Ask a follow-up...</span>
					</div>
				</div>
			</div>
		</div>

		<!-- ── Portfolio Intelligence ── -->
		<div data-child>
			<h3 class="text-[22px] font-medium text-white">Portfolio Intelligence</h3>
			<p class="mt-2 max-w-[480px] text-[15px] leading-[1.75] text-muted">
				Concentration risk, sector exposure, correlation analysis. Your portfolio reviewed every morning before you ask.
			</p>
			<div data-mockup class="mini-mockup">
				<div class="mini-mockup-bar">
					<span class="text-[10px] text-dim">Portfolio Overview</span>
					<span class="text-[11px] text-white">$24,831.40</span>
				</div>
				<div class="mini-mockup-body">
					<div class="space-y-3">
						<div class="flex items-center gap-2.5">
							<span class="w-10 text-[11px] text-white">NVDA</span>
							<div class="flex-1">
								<div class="h-[6px] w-full overflow-hidden rounded-full bg-raised">
									<div class="h-full rounded-full bg-[#444]" style="width:32%"></div>
								</div>
							</div>
							<span class="w-10 text-right text-[10px] text-muted">32.1%</span>
							<span class="w-12 text-right text-[11px] text-green">+18.4%</span>
						</div>
						<div class="flex items-center gap-2.5">
							<span class="w-10 text-[11px] text-white">AAPL</span>
							<div class="flex-1">
								<div class="h-[6px] w-full overflow-hidden rounded-full bg-raised">
									<div class="h-full rounded-full bg-[#444]" style="width:25%"></div>
								</div>
							</div>
							<span class="w-10 text-right text-[10px] text-muted">24.7%</span>
							<span class="w-12 text-right text-[11px] text-green">+4.2%</span>
						</div>
						<div class="flex items-center gap-2.5">
							<span class="w-10 text-[11px] text-white">MSFT</span>
							<div class="flex-1">
								<div class="h-[6px] w-full overflow-hidden rounded-full bg-raised">
									<div class="h-full rounded-full bg-[#444]" style="width:18%"></div>
								</div>
							</div>
							<span class="w-10 text-right text-[10px] text-muted">18.3%</span>
							<span class="w-12 text-right text-[11px] text-red">-1.8%</span>
						</div>
						<div class="flex items-center gap-2.5">
							<span class="w-10 text-[11px] text-white">AMZN</span>
							<div class="flex-1">
								<div class="h-[6px] w-full overflow-hidden rounded-full bg-raised">
									<div class="h-full rounded-full bg-[#444]" style="width:14%"></div>
								</div>
							</div>
							<span class="w-10 text-right text-[10px] text-muted">14.2%</span>
							<span class="w-12 text-right text-[11px] text-green">+7.6%</span>
						</div>
						<div class="flex items-center gap-2.5">
							<span class="w-10 text-[11px] text-muted">Cash</span>
							<div class="flex-1">
								<div class="h-[6px] w-full overflow-hidden rounded-full bg-raised">
									<div class="h-full rounded-full bg-[#333]" style="width:11%"></div>
								</div>
							</div>
							<span class="w-10 text-right text-[10px] text-muted">10.7%</span>
							<span class="w-12 text-right text-[11px]"></span>
						</div>
					</div>
					<div class="mt-4 flex items-start gap-2 rounded-lg border border-red/20 bg-red/5 px-3 py-2">
						<span class="text-red">&#x26A0;</span>
						<div>
							<div class="text-[10px] font-medium text-red">Risk Alert</div>
							<div class="mt-0.5 text-[10px] text-red/70">75% concentrated in top 3 holdings. Consider rebalancing.</div>
						</div>
					</div>
				</div>
			</div>
		</div>

		<!-- ── Insider Tracking ── -->
		<div data-child>
			<h3 class="text-[22px] font-medium text-white">Insider Tracking</h3>
			<p class="mt-2 max-w-[480px] text-[15px] leading-[1.75] text-muted">
				Every SEC Form 4 parsed and scored. Know which insider buys matter and which are noise.
			</p>
			<div data-mockup class="mini-mockup">
				<div class="mini-mockup-bar">
					<span class="text-[10px] text-dim">Insider Activity</span>
					<div class="flex gap-1.5">
						<span class="mini-dot"></span><span class="mini-dot"></span><span class="mini-dot"></span>
					</div>
				</div>
				<div class="mini-mockup-body space-y-0">
					<!-- Row 1 -->
					<div class="flex items-start gap-3 border-b border-border/60 py-3 first:pt-0">
						<div class="mt-0.5 flex h-[22px] w-[22px] shrink-0 items-center justify-center rounded-full bg-green/10 text-[10px] text-green">&#x25B2;</div>
						<div class="min-w-0 flex-1">
							<div class="flex items-baseline justify-between gap-2">
								<div class="flex items-baseline gap-1.5">
									<span class="text-[11px] text-white">Jensen Huang</span>
									<span class="text-[10px] text-dim">CEO</span>
								</div>
								<span class="text-[11px] text-white">$12.4M</span>
							</div>
							<div class="mt-0.5 flex items-center gap-2 text-[10px]">
								<span class="text-muted">NVDA</span>
								<span class="text-green">Buy</span>
								<span class="text-dim">&middot; Mar 24</span>
								<span class="text-dim">&middot; +2.1% since</span>
							</div>
							<div class="mt-1.5 flex items-center gap-1.5">
								<div class="h-[3px] w-16 overflow-hidden rounded-full bg-raised">
									<div class="h-full rounded-full bg-green" style="width:91%"></div>
								</div>
								<span class="text-[9px] text-green">91/100</span>
								<span class="text-[9px] text-dim">&mdash; strong signal</span>
							</div>
						</div>
					</div>
					<!-- Row 2 -->
					<div class="flex items-start gap-3 border-b border-border/60 py-3">
						<div class="mt-0.5 flex h-[22px] w-[22px] shrink-0 items-center justify-center rounded-full bg-green/10 text-[10px] text-green">&#x25B2;</div>
						<div class="min-w-0 flex-1">
							<div class="flex items-baseline justify-between gap-2">
								<div class="flex items-baseline gap-1.5">
									<span class="text-[11px] text-white">Lisa Su</span>
									<span class="text-[10px] text-dim">CEO</span>
								</div>
								<span class="text-[11px] text-white">$3.8M</span>
							</div>
							<div class="mt-0.5 flex items-center gap-2 text-[10px]">
								<span class="text-muted">AMD</span>
								<span class="text-green">Buy</span>
								<span class="text-dim">&middot; Mar 22</span>
								<span class="text-dim">&middot; +0.8% since</span>
							</div>
							<div class="mt-1.5 flex items-center gap-1.5">
								<div class="h-[3px] w-16 overflow-hidden rounded-full bg-raised">
									<div class="h-full rounded-full bg-green" style="width:84%"></div>
								</div>
								<span class="text-[9px] text-green">84/100</span>
								<span class="text-[9px] text-dim">&mdash; consistent buyer</span>
							</div>
						</div>
					</div>
					<!-- Row 3 -->
					<div class="flex items-start gap-3 py-3 last:pb-0">
						<div class="mt-0.5 flex h-[22px] w-[22px] shrink-0 items-center justify-center rounded-full bg-red/10 text-[10px] text-red">&#x25BC;</div>
						<div class="min-w-0 flex-1">
							<div class="flex items-baseline justify-between gap-2">
								<div class="flex items-baseline gap-1.5">
									<span class="text-[11px] text-white">Tim Cook</span>
									<span class="text-[10px] text-dim">CEO</span>
								</div>
								<span class="text-[11px] text-white">$8.2M</span>
							</div>
							<div class="mt-0.5 flex items-center gap-2 text-[10px]">
								<span class="text-muted">AAPL</span>
								<span class="text-red">Sell</span>
								<span class="text-dim">&middot; Mar 20</span>
								<span class="text-dim">&middot; -1.2% since</span>
							</div>
							<div class="mt-1.5 flex items-center gap-1.5">
								<div class="h-[3px] w-16 overflow-hidden rounded-full bg-raised">
									<div class="h-full rounded-full bg-red" style="width:34%"></div>
								</div>
								<span class="text-[9px] text-red">34/100</span>
								<span class="text-[9px] text-dim">&mdash; routine sale</span>
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>

		<!-- ── Verified Community ── -->
		<div data-child>
			<h3 class="text-[22px] font-medium text-white">Verified Community</h3>
			<p class="mt-2 max-w-[480px] text-[15px] leading-[1.75] text-muted">
				Link your brokerage. Share verified P&amp;L. See who&rsquo;s actually in the trade they&rsquo;re calling.
			</p>
			<div data-mockup class="mini-mockup">
				<div class="mini-mockup-bar">
					<span class="text-[10px] text-dim">Community Feed</span>
					<div class="flex gap-1.5">
						<span class="mini-dot"></span><span class="mini-dot"></span><span class="mini-dot"></span>
					</div>
				</div>
				<div class="mini-mockup-body space-y-0">
					<!-- User 1 -->
					<div class="border-b border-border/60 pb-3.5">
						<div class="flex items-center gap-2">
							<div class="flex h-6 w-6 items-center justify-center rounded-full bg-border text-[9px] text-muted">T</div>
							<span class="text-[11px] text-white">trader_0x7F</span>
							<span class="rounded border border-border bg-raised px-1.5 py-px text-[8px] text-muted">&#x2713; Verified</span>
							<span class="ml-auto text-[9px] text-[#444]">2h ago</span>
						</div>
						<p class="mt-2 ml-8 text-[11px] leading-[1.6] text-sub">&ldquo;Adding to NVDA here. Insider cluster buy + call flow alignment is too clean to ignore.&rdquo;</p>
						<div class="mt-2 ml-8 flex gap-4 text-[10px]">
							<span class="text-dim">Holds: <span class="text-[#999]">NVDA 32%</span></span>
							<span class="text-dim">90d: <span class="text-green">+24.1%</span></span>
						</div>
					</div>
					<!-- User 2 -->
					<div class="border-b border-border/60 py-3.5">
						<div class="flex items-center gap-2">
							<div class="flex h-6 w-6 items-center justify-center rounded-full bg-border text-[9px] text-muted">M</div>
							<span class="text-[11px] text-white">mktstructure</span>
							<span class="rounded border border-border bg-raised px-1.5 py-px text-[8px] text-muted">&#x2713; Verified</span>
							<span class="ml-auto text-[9px] text-[#444]">4h ago</span>
						</div>
						<p class="mt-2 ml-8 text-[11px] leading-[1.6] text-sub">&ldquo;Trimming TSLA before earnings. IV pricing in &plusmn;8% move, selling premium instead.&rdquo;</p>
						<div class="mt-2 ml-8 flex gap-4 text-[10px]">
							<span class="text-dim">Holds: <span class="text-[#999]">TSLA 8%</span></span>
							<span class="text-dim">90d: <span class="text-green">+11.7%</span></span>
						</div>
					</div>
					<!-- Unverified user -->
					<div class="pt-3.5 opacity-50">
						<div class="flex items-center gap-2">
							<div class="flex h-6 w-6 items-center justify-center rounded-full bg-raised text-[9px] text-[#444]">A</div>
							<span class="text-[11px] text-dim">anon_alpha</span>
							<span class="rounded border border-border/50 px-1.5 py-px text-[8px] text-[#444]">Unverified</span>
						</div>
						<p class="mt-2 ml-8 text-[11px] text-dim">&ldquo;PLTR to $100 easy 🚀🚀🚀&rdquo;</p>
						<div class="mt-1.5 ml-8 text-[9px] text-[#444]">No linked brokerage &mdash; position not verified</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</section>

<!-- ══════════════════════ 5. SOCIAL PROOF ══════════════════════ -->

<section use:reveal data-reveal class="mx-auto max-w-[1100px] px-6 py-20 md:px-10 md:py-[140px]">
	<p data-child class="font-mono text-[12px] uppercase tracking-[0.2em] text-dim">// THE COMMUNITY</p>
	<h2 data-child class="mt-5 max-w-[700px] text-[clamp(24px,3.5vw,36px)] leading-[1.2] font-normal text-heading">
		Not another dead server with 50,000 members and zero signal.
	</h2>
	<p data-child class="mt-5 max-w-[580px] text-[16px] leading-[1.75] text-muted">
		KAOS is building the investing community that should have existed years ago &mdash; one where every member&rsquo;s positions are verified, the AI drops a briefing every morning, and the culture rewards substance over clout.
	</p>
</section>

<!-- Marquee -->
<div
	class="overflow-hidden py-8"
	style="mask-image: linear-gradient(to right, transparent, black 80px, black calc(100% - 80px), transparent); -webkit-mask-image: linear-gradient(to right, transparent, black 80px, black calc(100% - 80px), transparent);"
>
	<div class="marquee-track inline-flex whitespace-nowrap">
		<span class="font-mono text-[13px] text-dim">{marqueeQuotes}</span>
		<span class="font-mono text-[13px] text-dim">{marqueeQuotes}</span>
	</div>
</div>

<!-- ══════════════════════ 6. FINAL CTA ══════════════════════ -->

<section use:reveal data-reveal class="py-20 text-center md:py-[140px]">
	<h2 data-child class="font-pixel text-[clamp(28px,5vw,48px)] leading-[1.2] text-white">
		Build starts in Discord.
	</h2>
	<p data-child class="mx-auto mt-5 max-w-[460px] text-[17px] leading-[1.7] text-muted">
		We&rsquo;re building the platform with a small group of early members. Join the server and shape what KAOS becomes.
	</p>
	<div data-child class="mt-7">
		<a href={DISCORD_URL} target="_blank" rel="noopener" class="cta-btn text-[16px]" style="padding: 16px 36px;">Join the Discord</a>
	</div>
	<p data-child class="mt-4 font-mono text-[11px] text-faint">Free. No email required.</p>
</section>

<!-- ══════════════════════ 7. FOOTER ══════════════════════ -->

<footer class="mx-auto max-w-[1100px] border-t border-divider px-6 py-12 md:px-10">
	<div class="flex flex-col items-start justify-between gap-4 sm:flex-row sm:items-center">
		<span class="font-mono text-[11px] text-faint">K A O S &copy; 2026</span>
		<div class="flex gap-6 font-mono text-[11px] text-faint">
			<a href={DISCORD_URL} target="_blank" rel="noopener" class="transition-colors hover:text-[#cccccc]">Discord</a>
			<a href={TWITTER_URL} target="_blank" rel="noopener" class="transition-colors hover:text-[#cccccc]">X</a>
		</div>
	</div>
	<p class="mt-4 text-center font-mono text-[10px] text-ghost">
		KAOS does not provide financial advice. All investments carry risk.
	</p>
</footer>
