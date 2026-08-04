export default {
  async fetch(request, env) {
    // Ignore browser cache
    const headers = {
      "Content-Type": "image/svg+xml",
      "Cache-Control": "no-store, no-cache, must-revalidate, max-age=0"
    };

    // Counter key for your GitHub profile
    const KEY = "devotedmikae_profile_views";

    // Read current count
    let count = await env.VIEWS.get(KEY);

    // Start at 690
    if (count === null) {
      count = 690;
    } else {
      count = Number(count) + 1;
    }

    // Save updated count
    await env.VIEWS.put(KEY, String(count));

    const label = "Profile Views";
    const value = count.toString();

    const labelWidth = 90;
    const valueWidth = Math.max(45, value.length * 8 + 14);
    const totalWidth = labelWidth + valueWidth;

    const svg = `
<svg xmlns="http://www.w3.org/2000/svg" width="${totalWidth}" height="20">
  <linearGradient id="gradient" x2="0" y2="100%">
    <stop offset="0" stop-color="#ffffff" stop-opacity=".1"/>
    <stop offset="1" stop-opacity=".1"/>
  </linearGradient>

  <rect rx="4" width="${labelWidth}" height="20" fill="#555"/>
  <rect rx="4" x="${labelWidth}" width="${valueWidth}" height="20" fill="#246E78"/>
  <path fill="#246E78" d="M${labelWidth} 0h4v20h-4z"/>
  <rect rx="4" width="${totalWidth}" height="20" fill="url(#gradient)"/>

  <g fill="#ffffff"
     font-family="Verdana,Geneva,DejaVu Sans,sans-serif"
     font-size="11"
     text-anchor="middle">

    <text x="${labelWidth / 2}" y="14">
      ${label}
    </text>

    <text x="${labelWidth + valueWidth / 2}" y="14">
      ${value}
    </text>

  </g>
</svg>`;

    return new Response(svg, { headers });
  }
};
