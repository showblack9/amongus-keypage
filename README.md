# Among Us Daily Key Page

This repository hosts the daily key page for the Among Us hack.

## How it works

- The key changes every day based on the current UTC date and a secret.
- The secret used here is: `showblack_secret`.
- The page computes the key using SHA-256 in JavaScript.
- Users copy the key from this page and paste it into the hack menu.
- The hack menu validates the key locally using the same secret and date.

## Using the Key in C#

Here is a sample C# class to generate and validate the key:

```csharp
using System;
using System.Security.Cryptography;
using System.Text;

public static class KeySystem
{
    private const string secret = "showblack_secret";

    public static string GenerateTodayKey()
    {
        string date = DateTime.UtcNow.ToString("yyyy-MM-dd");
        string raw = date + secret;

        using (SHA256 sha256 = SHA256.Create())
        {
            byte[] bytes = sha256.ComputeHash(Encoding.UTF8.GetBytes(raw));
            StringBuilder builder = new StringBuilder();
            foreach (var b in bytes)
                builder.Append(b.ToString("x2"));
            return builder.ToString();
        }
    }

    public static bool IsValidKey(string input)
    {
        return input == GenerateTodayKey();
    }
}
