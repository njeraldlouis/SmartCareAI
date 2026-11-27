# SmartCare AI

A .NET 10 and Blazor Server demo that turns a fictional Tamil, Tanglish, or English intake note into a structured English draft for staff review using Microsoft Foundry.

Built by [Jerald Felix](https://sessionize.com/jerald-felix/) for .NET Conf Coimbatore 2026.

> Educational demo only. Use fictional data. This app does not diagnose, triage, route patients, or book appointments. Do not use it in real care workflows.

## How it works

1. The Blazor page sends the fictional note to a server-side C# service.
2. The service calls a deployed Foundry chat model and requests a structured JSON response.
3. The app shows the English summary, reported symptoms, duration, and details that may need staff review. Review the draft; structured output does not guarantee factual accuracy.

## Run locally

You need the [.NET 10 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/10.0) and an [Azure OpenAI deployment in Microsoft Foundry](https://ai.azure.com/) that supports [structured outputs](https://learn.microsoft.com/en-us/azure/foundry/openai/how-to/structured-outputs).

From the folder containing `SmartCareAI.csproj`, set the following local development values (macOS, zsh). Use your own resource endpoint and exact deployment name.

```zsh
dotnet restore
dotnet user-secrets init
dotnet user-secrets set 'Foundry:Endpoint' 'https://YOUR-RESOURCE.openai.azure.com/openai/v1/'
dotnet user-secrets set 'Foundry:Deployment' 'YOUR-DEPLOYMENT-NAME'
```

Run this line by itself, then paste the resource API key at the hidden prompt and press Enter:

```zsh
read -rs 'key?Foundry resource key: '; echo
```

```zsh
[[ -n "$key" ]] && dotnet user-secrets set 'Foundry:ApiKey' "$key"
unset key
ASPNETCORE_ENVIRONMENT=Development dotnet run --no-launch-profile --urls http://localhost:5078
```

Open [localhost:5078](http://localhost:5078) and select **Draft intake note**. Use the resource-level OpenAI v1 endpoint rather than the Foundry project endpoint. Keep credentials in [.NET User Secrets](https://learn.microsoft.com/en-us/aspnet/core/security/app-secrets?view=aspnetcore-10.0), not in the source or `appsettings*.json`.

## Try it

> Yesterday evening lendhu cough irukku. Appointment book panna vandhen.

The draft should capture the reported cough, its onset, and the request for an appointment. Wording may vary; review the draft against the original input.

## Key files

- [`Components/Pages/Home.razor`](Components/Pages/Home.razor): interactive demo page
- [`Services/IntakeService.cs`](Services/IntakeService.cs): Foundry call, instructions, and JSON schema
- [`Program.cs`](Program.cs): app startup and service registration
