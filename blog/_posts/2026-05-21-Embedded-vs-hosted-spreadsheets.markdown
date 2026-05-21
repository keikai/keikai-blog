---
layout: post
title:  "Embedded vs hosted spreadsheets"
date:   2026-05-21
categories: "Guide"
excerpt: "Keikai vs Google Sheets and Microsoft 365: Embedded Component or Hosted Service?"

index-intro: "
Keikai vs Google Sheets and Microsoft 365: Embedded Component or Hosted Service?
"
image: "2026-05-embedded_hosted/embedded-vs-hosted-sheets-banner.png"
tags: guide spreadsheet embedded hosted
author: "Jean Yen"
authorImg: "/images/author/jean.jpg"
authorDesc: "Marketing, Keikai."

imgDir: "2026-05-embedded_hosted"
javadoc: "http://keikai.io/javadoc/latest/"
---
![]({{ site.baseurl }}/images/{{page.imgDir}}/embedded-vs-hosted-sheets-banner.png)

# Keikai vs Google Sheets and Microsoft 365: Embedded Component or Hosted Service?

All three of these products let people work with spreadsheets in a browser, and at first glance they can look similar enough that teams compare them feature by feature — pivot tables, charts, formula support, or .xlsx compatibility.

But the more important distinction is architectural. Keikai is an embedded component you install into your application. Google Sheets and Microsoft 365 are hosted services your users access separately. Those are very different product models, and they fit very different use cases.

## The architectural distinction

A hosted spreadsheet like Google Sheets is a complete SaaS application. The vendor runs the servers, stores the data, ships the UI, manages user accounts, and handles updates. To use it, your users go to Google's or Microsoft's environment — either directly or through an embedded iframe — sign in, and work there.

An embedded spreadsheet component is a library you install into your application. The component renders a spreadsheet UI inside the rest of your app. The data lives in your stack — your database, your file storage, your servers. The user is signed into your application, not Google's or Microsoft's. The spreadsheet becomes one part of the overall application UI.

The same end-user task — editing cells, writing formulas, saving results — can be done in either model. But almost everything around that task is different.

| Concern | Hosted service (Google Sheets, Microsoft 365) | Embedded component (Keikai) |
|---|---|---|
| Where data is stored | Vendor cloud | Your servers / your database |
| Where users sign in | Vendor identity system (or SSO into it) | Your application's authentication |
| URL the user sees | sheets.google.com / office.com (or vendor iframe) | Your application's domain |
| UI shell and branding | Vendor-controlled | Your UI and branding |
| Who handles updates | Vendor | You |
| Who handles uptime | Vendor | You |
| Integration with business logic | API integrations | Runs within your application stack |

Almost every practical difference between these approaches follows from one of these architectural decisions.

## When an embedded component is the right fit

An embedded spreadsheet component makes sense when the spreadsheet is part of your application, not a separate destination users visit independently.

### The spreadsheet is part of the product experience

If you're building a financial modeling SaaS, ERP module, billing system, cap-table tool, or scientific analysis platform, the spreadsheet is often one screen inside a larger workflow. Sending users out to a separate spreadsheet service and back into your app can complicate UX, permissions, and data synchronization.

With an embedded component, the spreadsheet integrates with the same authentication, navigation, and backend systems as the rest of the application.

### The data needs to stay in your environment

Some industries — including healthcare, finance, government, and defense — may require data to remain inside customer-controlled infrastructure. Data residency policies, regulatory requirements, internal security reviews, or enterprise procurement rules may limit the use of third-party hosted services.

An embedded component runs on infrastructure you control and connects directly to your own storage and services.

### You're supporting on-premises or isolated deployments

Some customers require software to run entirely within their own environment, including deployments with no outbound internet access. Hosted spreadsheet services are generally not designed for these environments, while embedded components can be deployed as part of the application's own infrastructure.

### The spreadsheet needs deep backend integration

This is one of the biggest differences between the models.

Keikai, for example, allows developers to write custom spreadsheet functions in Java that connect directly to existing services — retrieving live pricing data, performing calculations, or loading customer information from backend systems. Applications can also react to spreadsheet events server-side and synchronize changes directly with databases or workflows.

Hosted services can integrate through APIs as well, but the integration model is different and typically requires external API communication, authentication handling, and synchronization logic between systems.

### You need full control of the user experience

White-labeled enterprise software and customer-facing applications often need the spreadsheet UI to match the rest of the product experience. Using a hosted spreadsheet interface inside an iframe introduces another product's menus, branding, permissions model, and navigation patterns into your application.

If the goal is to make spreadsheets feel like a native part of your application, an embedded component is often a better fit than embedding a hosted spreadsheet UI through an iframe.

Examples of applications that commonly benefit from this model include ERP and CRM systems, accounting platforms, insurance and actuarial tools, tax software, financial-planning applications, scientific analysis platforms, and internal enterprise systems.

### See it in code: load from database, edit, save back

To make the architectural difference concrete, here's a realistic scenario — adapted from the [Keikai database tutorial](https://doc.keikai.io/tutorial/database) — that every business application eventually runs into. The user opens a workbook showing trade records pulled from a database. They edit some cells. They click **Save to Database** to persist the changes, or **Load from Database** to refresh.

**With Keikai (embedded component):**

The spreadsheet is wired into the controller as a Java object alongside any other UI component. The data service is a normal injected dependency. The Save and Load buttons trigger methods on the same controller, in the same process.

```java
public class TradeController extends SelectorComposer<Component> {

    @Wire private Spreadsheet ss;
    private TradeService dataService = new TradeService();

    @Listen("onClick = #load")
    public void load() {
        List<Trade> trades = dataService.findAll();
        Sheet sheet = ss.getSelectedSheet();
        for (int i = 0; i < trades.size(); i++) {
            populate(sheet, i + 1, trades.get(i));
        }
    }

    @Listen("onClick = #save")
    public void save() {
        Sheet sheet = ss.getSelectedSheet();
        List<Trade> trades = new ArrayList<>();
        for (int row = 1; row <= LAST_ROW; row++) {
            trades.add(extract(sheet, row));
        }
        dataService.save(trades);   // same JVM, same transaction
    }

    private void populate(Sheet sheet, int row, Trade t) {
        Ranges.range(sheet, row, 0).setCellValue(t.getId());
        Ranges.range(sheet, row, 1).setCellValue(t.getType());
        Ranges.range(sheet, row, 2).setCellValue(t.getSalesPerson());
        Ranges.range(sheet, row, 3).setCellValue(t.getSale());
    }

    private Trade extract(Sheet sheet, int row) {
        Trade t = new Trade();
        t.setId((int) Ranges.range(sheet, row, 0).getCellData().getDoubleValue());
        t.setType(Ranges.range(sheet, row, 1).getCellEditText());
        t.setSalesPerson(Ranges.range(sheet, row, 2).getCellEditText());
        t.setSale((int) Ranges.range(sheet, row, 3).getCellData().getDoubleValue());
        return t;
    }
}
```

That's the entire integration. Nothing crosses the network. `TradeService` is a direct dependency. Your security context is in scope. Your transaction manager covers the save like any other database operation in your app.

**With Google Sheets (hosted service):**

The data lives in your database. The UI for editing it lives in Google's cloud. Every read and every write has to cross that boundary. You'll need a Google Cloud project with the Sheets API enabled, a service account or OAuth client, and the target spreadsheet shared with that account.

```java
// One-time setup
GoogleCredentials credentials = GoogleCredentials
    .fromStream(new FileInputStream("/credentials.json"))
    .createScoped(List.of(SheetsScopes.SPREADSHEETS));

Sheets sheets = new Sheets.Builder(
        GoogleNetHttpTransport.newTrustedTransport(),
        GsonFactory.getDefaultInstance(),
        new HttpCredentialsAdapter(credentials))
    .setApplicationName("Trade Tool")
    .build();
```

Load — query your database, then push the rows into the sheet over the network:

```java
@PostMapping("/load")
public void load(@RequestParam String spreadsheetId) throws IOException {
    List<Trade> trades = dataService.findAll();

    List<List<Object>> values = trades.stream()
        .map(t -> List.<Object>of(t.getId(), t.getType(), t.getSalesPerson(), t.getSale()))
        .collect(Collectors.toList());

    sheets.spreadsheets().values()
        .update(spreadsheetId, "Trades!A2", new ValueRange().setValues(values))
        .setValueInputOption("RAW")
        .execute();   // network call to Google
}
```

Save — read the rows back from the sheet over the network, then persist:

```java
@PostMapping("/save")
public void save(@RequestParam String spreadsheetId) throws IOException {
    ValueRange response = sheets.spreadsheets().values()
        .get(spreadsheetId, "Trades!A2:D")
        .execute();   // network call to Google

    List<Trade> trades = response.getValues().stream()
        .map(row -> {
            Trade t = new Trade();
            t.setId(Integer.parseInt(row.get(0).toString()));
            t.setType(row.get(1).toString());
            t.setSalesPerson(row.get(2).toString());
            t.setSale(Integer.parseInt(row.get(3).toString()));
            return t;
        })
        .collect(Collectors.toList());

    dataService.save(trades);
}
```

And then there's everything the code doesn't show: refreshing OAuth tokens, retrying transient API failures, staying within quotas (roughly 60 writes and 300 reads per minute per project), handling the race where a user is still editing a cell while your Save call is reading from it, and accepting that the data has now travelled to Google's servers and back even though it was always meant to live in your database.

The two are very different shapes of code. The embedded version is one controller class with two methods, reading and writing local objects in the same JVM as the rest of your app. The hosted version is a distributed integration with a third-party API sitting in the middle of every read and every write — even though the data originates in your database and ends right back in your database.

## When a hosted service is the right fit

If the spreadsheet itself is the primary workspace — for example, budgeting, reporting, collaboration, or sharing information with external users — hosted services are often the simpler and more practical option.

Google Sheets and Excel for the web are mature collaboration platforms with broad feature sets. Real-time collaboration, sharing, comments, version history, mobile access, and ecosystem integrations are available as part of the platform. Most users are already familiar with these tools, and organizations can often adopt them quickly without building or maintaining spreadsheet infrastructure themselves.

For many general productivity and collaboration scenarios, that convenience is a major advantage.

## Browser-based spreadsheets and VBA

One limitation shared by Google Sheets, Microsoft 365, and embedded browser-based spreadsheet components like Keikai is that VBA macros do not run in the browser.

Microsoft 365 preserves VBA macros in uploaded workbooks but does not execute them in Excel for the web. Google Sheets uses Apps Script instead of VBA. Embedded components like Keikai typically replace VBA with server-side application logic and custom functions written in the host language.

For teams migrating from desktop Excel, macro-heavy workbooks usually require some rewrite effort regardless of platform. Other desktop-specific technologies, such as ActiveX controls, COM add-ins, and OLE automation, should also be reviewed during migration planning.

## A short decision framework

Walk through these questions in order. The first "yes" is often the clearest direction.

- Is the spreadsheet part of a larger application workflow? → Embedded component
- Does the data need to remain inside your infrastructure for compliance or security reasons? → Embedded component
- Are you supporting on-premises or isolated deployments? → Embedded component
- Does the spreadsheet need direct integration with backend services or business logic? → Embedded component
- Is the spreadsheet itself the main workspace for collaboration and sharing? → Hosted service
- Is this primarily a general productivity use case with no major integration or infrastructure constraints? → Hosted service


## Architectural fit matters more than features

If the spreadsheet is part of an application you're building, an embedded component is often the better fit. If the spreadsheet itself is the destination — collaboration, sharing, and general productivity work — hosted services usually provide a faster path with less infrastructure to manage.

Compliance requirements, infrastructure control, integration depth, and deployment flexibility tend to favor the embedded model. Collaboration tooling, rapid adoption, and external sharing tend to favor hosted services.

Most feature-level comparisons matter less than choosing the model that matches the role the spreadsheet plays in the overall system.
