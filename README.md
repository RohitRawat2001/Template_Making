# Template_Making 
How to make template using html (ftl) . send email to with html template. 

# Template using loop 
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ROMS Report</title>
</head>

<body
    style="background-color: #e9ecef; font-family: Arial, sans-serif; margin: 0; padding: 0; width: 100vw; height: 100vh; display: flex; align-items: center; justify-content: center;">

    <div
        style="width: 100vw; height: 100vh; background: white; box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1); padding: 40px; overflow: auto;">

        <div style="display: flex; align-items: center; margin-bottom: 20px;">
            <img src="https://sit.rtl.net.au/assets/img/ROMS%20logo-423.png" alt="ROMS Logo"
                style="width: 50px; height: 50px; margin-right: 12px; background-color: #c2410c;" />
            <h1 style="font-weight: 700; color: #c2410c; font-size: 24px;">ROMS</h1>
        </div>

        <div
            style="width: 75%; border: 1px solid #dcdcdc; border-radius: 4px; padding: 20px; background-color: #ffffff;">
            <h1 style="font-size: 24px;">Light Vehicles Not Pre-Started in the Last Month</h1>
            <div style="font-size: 14px; margin-bottom: 24px;">
                <p>Hello,</p>
                <p>
                    The attached report lists Light Vehicles (LVs) not pre-started in
                    the past month, categorized by project for clarity. Please review
                    and address any vehicles requiring attention.
                </p>
            </div>

         <#list dataList as model>
<section>
    <p style="font-size: 14px;"><span style="color: grey;">Project</span> ${model.projectName}</p>
    <div style="overflow-x: auto; margin-bottom: 20px;">
        <table style="width: 100%; background-color: #f8fafc; border-collapse: collapse; text-align: center;">
            <thead>
                <tr>
                    <th style="background-color: #e7eef3; font-size: 12px; color: #4a5568; padding: 10px;">Asset</th>
                    <th style="background-color: #e7eef3; font-size: 12px; padding: 10px;">Last Prestart</th>
                    <th style="background-color: #e7eef3; font-size: 12px; padding: 10px;">Last Operator</th>
                    <th style="background-color: #e7eef3; font-size: 12px; padding: 10px;">Last Service Date</th>
                    <th style="background-color: #e7eef3; font-size: 12px; padding: 10px;">Service Due Date</th>
                    <th style="background-color: #e7eef3; font-size: 12px; padding: 10px;">Service Due SMU</th>
                    <th style="background-color: #e7eef3; font-size: 12px; padding: 10px;">Current SMU</th>
                    <th style="background-color: #e7eef3; font-size: 12px; padding: 10px;">Open Defect</th>
                </tr>
            </thead>
            <tbody>
                <#list model.assetList as asset>
                <tr>
                    <td style="font-size: 12px;">
                        <div style="display: flex; align-items: center; justify-content: center;">
                            <div style="width: 30px; height: 30px; border-radius: 50%; overflow: hidden; margin-right: 8px;">
                                <img src="${asset.imageOperator}" alt="Operator Image" style="width: 100%; height: 100%; object-fit: cover;" />
                            </div>
                            <div style="flex-direction: column;">
                                <span style="color: rgb(43 163 201 / 86%); font-weight: 600;">${asset.assetNo}</span><br />
                                <span style="color: grey; font-size: 10px;">${asset.assetModel}</span>
                            </div>
                        </div>
                    </td>
                    <td style="font-size: 12px;">${asset.lastPrestartDate} <br /><span style="color: #f56565; font-size: 10px;">${asset.lastPrestartDays}</span></td>
                    <td style="font-size: 12px;">
                        <div style="display: flex; align-items: center; justify-content: center;">
                            <div style="width: 30px; height: 30px; border-radius: 50%; overflow: hidden; margin-right: 8px;">
                                <img src="${asset.imageOperator}" alt="Operator Image" style="width: 100%; height: 100%; object-fit: cover;" />
                            </div>
                            <b>${asset.lastOperator}</b>
                        </div>
                    </td>
                    <td style="font-size: 12px;">${asset.lastServiceDate}</td>
                    <td style="font-size: 12px;">${asset.serviceDueDate}<br /><span style="color: grey; font-size: 10px;">${asset.overduedays}&emsp;</span><span style="color: #f56565; font-size: 10px;">${asset.overdue}</span></td>
                    <td style="font-size: 12px;">${asset.serviceDueSmu}<br /><span style="color: grey; font-size: 10px;">${asset.smu}&emsp;</span><span style="color: #f56565; font-size: 10px;">${asset.smuNo}</span></td>
                    <td style="font-size: 12px;">${asset.currentSmu}</td>
                    <td style="font-size: 12px;">${asset.openDefect}</td>
                </tr>
                </#list>
            </tbody>
        </table>
    </div>
</section>
</#list>

        </div>

        <footer style="margin-top: 20px; color: grey; font-size: 10px">
            <p>ITL, Mining and Earthworks Pty Ltd.</p>
            <p>Eastern Road, Valliton Vic 3825</p>
            <p>PO Box 296, Mic Vic 3828</p>
            <p>© Dragon and Peaches</p>
        </footer>

    </div>

</body>

</html>
```


# service email :

```
    @Async
    public void sendEmailIndexEmailTemplate(String toMail,Map<String,Object> model,String subject) throws Exception{
        MimeMessage message = javaMailSender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(message,MimeMessageHelper.MULTIPART_MODE_MIXED_RELATED,
                StandardCharsets.UTF_8.name());
        Template template = freemarkerConfiguration.getTemplate("indexEmailTemplate.ftl");
        String html = FreeMarkerTemplateUtils.processTemplateIntoString(template, model);
        helper.setTo(toMail);
        helper.setText(html,true);
        helper.setSubject(subject);
        helper.setFrom(new InternetAddress(senderEmail, "ROMS Smart Mine Operations"));
        javaMailSender.send(message);
    }
```


#controller :

```
 @PostMapping(value = "/sendIndex", consumes = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
    public ResponseEntity<?> sendIndexfileEmail() {
        try {
            Map<String, Object> model = new HashMap<>();
            List<Map<String, Object>> dataList = new ArrayList<>();

            // Project 1: "Mission45" with multiple assets
            Map<String, Object> project1 = new HashMap<>();
            project1.put("projectName", "Mission45");

            List<Map<String, Object>> assetList1 = new ArrayList<>();

            Map<String, Object> asset1 = new HashMap<>();
            asset1.put("assetNo", "VFB525");
            asset1.put("assetModel", "Toyota LC D/Cab");
            asset1.put("lastPrestartDate", "17/01/2025");
            asset1.put("lastPrestartDays", "33 days ago");
            asset1.put("lastOperator", "David Warren");
            asset1.put("lastServiceDate", "15/01/2025");
            asset1.put("serviceDueDate", "133791");
            asset1.put("overdue", "Overdue");
            asset1.put("overduedays", "33");
            asset1.put("serviceDueSmu", "1245791");
            asset1.put("smu", "overdue MU");
            asset1.put("smuNo", "321");
            asset1.put("currentSmu", "5");
            asset1.put("openDefect", "5");
            asset1.put("imageOperator", "https://images.unsplash.com/photo-1591738802175-709fedef8288?q=80&w=1887&auto=format&fit=crop");
            assetList1.add(asset1);

            Map<String, Object> asset2 = new HashMap<>();
            asset2.put("assetNo", "VFB526");
            asset2.put("assetModel", "Ford Ranger");
            asset2.put("lastPrestartDate", "18/01/2025");
            asset2.put("lastPrestartDays", "30 days ago");
            asset2.put("lastOperator", "John Doe");
            asset2.put("lastServiceDate", "14/01/2025");
            asset2.put("serviceDueDate", "135000");
            asset2.put("overdue", "Not Overdue");
            asset2.put("overduedays", "0");
            asset2.put("serviceDueSmu", "1250000");
            asset2.put("smu", "normal MU");
            asset2.put("smuNo", "322");
            asset2.put("currentSmu", "6");
            asset2.put("openDefect", "3");
            asset2.put("imageOperator", "https://images.unsplash.com/photo-1591738802175-709fedef8288?q=80&w=1887&auto=format&fit=crop");
            assetList1.add(asset2);

            project1.put("assetList", assetList1);
            dataList.add(project1);

            // Project 2: "Mission46" with multiple assets
            Map<String, Object> project2 = new HashMap<>();
            project2.put("projectName", "Mission46");

            List<Map<String, Object>> assetList2 = new ArrayList<>();

            Map<String, Object> asset3 = new HashMap<>();
            asset3.put("assetNo", "VFB527");
            asset3.put("assetModel", "Honda CRV");
            asset3.put("lastPrestartDate", "20/01/2025");
            asset3.put("lastPrestartDays", "28 days ago");
            asset3.put("lastOperator", "Alice Smith");
            asset3.put("lastServiceDate", "19/01/2025");
            asset3.put("serviceDueDate", "135500");
            asset3.put("overdue", "Not Overdue");
            asset3.put("overduedays", "0");
            asset3.put("serviceDueSmu", "1260000");
            asset3.put("smu", "normal MU");
            asset3.put("smuNo", "323");
            asset3.put("currentSmu", "4");
            asset3.put("openDefect", "2");
            asset3.put("imageOperator", "https://images.unsplash.com/photo-1591738802175-709fedef8288?q=80&w=1887&auto=format&fit=crop");
            assetList2.add(asset3);

            Map<String, Object> asset4 = new HashMap<>();
            asset4.put("assetNo", "VFB528");
            asset4.put("assetModel", "Nissan Patrol");
            asset4.put("lastPrestartDate", "22/01/2025");
            asset4.put("lastPrestartDays", "25 days ago");
            asset4.put("lastOperator", "Bob Johnson");
            asset4.put("lastServiceDate", "21/01/2025");
            asset4.put("serviceDueDate", "136000");
            asset4.put("overdue", "Overdue");
            asset4.put("overduedays", "25");
            asset4.put("serviceDueSmu", "1270000");
            asset4.put("smu", "overdue MU");
            asset4.put("smuNo", "324");
            asset4.put("currentSmu", "3");
            asset4.put("openDefect", "4");
            asset4.put("imageOperator", "https://images.unsplash.com/photo-1591738802175-709fedef8288?q=80&w=1887&auto=format&fit=crop");
            assetList2.add(asset4);

            project2.put("assetList", assetList2);
            dataList.add(project2);

            // Add the data list to the model to pass to the template
            model.put("dataList", dataList);

            // Send the email with the combined data list
            mailSenderService.sendEmailIndexEmailTemplate("rohitrawat5172@gmail.com", model, "Light Vehicles Not Pre-Started Report");

            return ResponseEntity.ok().body(Map.of("status", "Emails sent successfully"));

        } catch (Exception e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(Map.of("error", "Failed to send emails"));
        }
    }
```


# output :

![dfasdfasdf](https://github.com/user-attachments/assets/bbdfd753-b56c-4f04-a4f9-a592c84ac35d)


