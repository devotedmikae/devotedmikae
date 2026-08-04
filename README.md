export default {
  async fetch(request, env) {
    let count = await env.COUNTER.get("views");

    if (!count) {
      count = 690;
    } else {
      count = Number(count) + 1;
    }

    await env.COUNTER.put("views", String(count));

    const svg = `
<svg xmlns="http://www.w3.org/2000/svg" width="145" height="20">
  <rect rx="10" width="145" height="20" fill="#246E78"/>
  <text x="10" y="14" fill="#ffffff" font-family="Verdana" font-size="11">
    Profile Views ${count}
  </text>
</svg>`;

    return new Response(svg, {
      headers: {
        "Content-Type": "image/svg+xml",
        "Cache-Control": "no-store"
      }
    });
  }
}
