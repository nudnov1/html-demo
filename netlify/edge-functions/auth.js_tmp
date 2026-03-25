export default async (request, context) => {
  const authHeader = request.headers.get("Authorization");

  if (!authHeader) {
    return new Response("Unauthorized", {
      status: 401,
      headers: {
        "WWW-Authenticate": 'Basic realm="Secure Area"',
      },
    });
  }

  const [scheme, encoded] = authHeader.split(" ");
  if (scheme !== "Basic") {
    return new Response("Unauthorized", { status: 401 });
  }

  try {
    const decoded = atob(encoded);
    const [username, password] = decoded.split(":");

    // Get credentials from environment variables
    const validUser = Deno.env.get("BASIC_AUTH_USER") || "admin";
    const validPass = Deno.env.get("BASIC_AUTH_PASS") || "password";

    if (username === validUser && password === validPass) {
      return context.next();
    }
  } catch (e) {
    // Fail silently or log
  }

  return new Response("Unauthorized", {
    status: 401,
    headers: {
      "WWW-Authenticate": 'Basic realm="Secure Area"',
    },
  });
};

export const config = { path: "/*" };
