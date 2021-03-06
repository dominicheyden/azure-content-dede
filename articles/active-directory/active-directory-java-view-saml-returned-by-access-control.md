<properties
    pageTitle="Anzeigen der vom Azure Access Control Service zurückgegebenen SAML (Java)"
    description="Erfahren Sie, wie Sie die vom Access Control Service in auf Azure gehosteten Java-Anwendungen zurückgegebene SAML anzeigen."
	services="active-directory" 
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="" />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="03/04/2016" 
    ms.author="robmcm" />

# Anzeigen der vom Azure Access Control Service zurückgegebenen SAML

Diese Anleitung erklärt, wie Sie die Security Assertion Markup Language (SAML) anzeigen können, die vom Azure Access Control Service (ACS) an Ihre Anwendung zurückgegeben wurde. Diese Anleitung baut auf das Thema [Authentifizieren von Webbenutzern mit dem Azure Access Control Service über Eclipse][] auf und enthält Code zum Anzeigen der SAML-Informationen. Die fertige Anwendung sieht etwa wie folgt aus.

![Beispiel-SAML-Ausgabe][saml_output]

Weitere Informationen zum ACS finden Sie im Abschnitt [Nächste Schritte](#next_steps).

> [AZURE.NOTE]
Der Azure Access Control Service-Filter ist eine Technologievorschau für die Community. Als Vorabversion wird diese Software von Microsoft nicht offiziell unterstützt.

## Voraussetzungen

Um die Aufgaben in dieser Anleitung erledigen zu könne, müssen Sie zunächst das Beispiel unter [Authentifizieren von Webbenutzern mit dem Azure Access Control Service über Eclipse][] abschließen und als Ausgangspunkt für dieses Lernprogramm verwenden.

## Fügen Sie die JspWriter-Bibliothek zu Ihrem Buildpfad und zu Ihrer Bereitstellungs-Assembly hinzu

Fügen Sie die Bibliothek mit der Klasse **javax.servlet.jsp.JspWriter** zu Ihrem Buildpfad und zu Ihrer Bereitstellungs-Assembly hinzu. Falls Sie Tomcat verwenden, ist diese Klasse in der Datei **jsp-api.jar** enthalten, die sich im Apache **lib**-Ordner befindet.

1. Klicken Sie im Project Explorer in Eclipse mit der rechten Maustaste auf **MyACSHelloWorld**, anschließend auf **Build Path**, auf **Configure Build Path**, auf die Registerkarte **Libraries** und zuletzt auf **Add External JARs**.
2. Navigieren Sie im Dialogfeld **JAR Selection** zum entsprechenden JAR, wählen Sie das JAR aus und klicken Sie auf **Open**.
3. Klicken Sie im immer noch geöffneten **Properties for MyACSHelloWorld**-Dialogfeld auf **Deployment Assembly**.
4. Klicken Sie im Dialogfeld **Web Deployment Assembly** auf **Add**.
5. Klicken Sie im Dialogfeld **New Assembly Directive** auf **Java Build Path Entries** und anschließend auf **Next**.
6. Wählen Sie die entsprechende Bibliothek aus und klicken Sie auf **Finish**.
7. Klicken Sie auf **OK**, um das Dialogfeld **Properties for MyACSHelloWorld** zu schließen.

## Ändern der JSP-Datei zur Anzeige von SAML

Fügen Sie den folgenden Code in **index.jsp** ein.

	<%@ page language="java" contentType="text/html; charset=UTF-8"
	    pageEncoding="UTF-8"%>
	    <%@ page import="javax.xml.parsers.*"
	             import="javax.xml.transform.*"
	             import="org.w3c.dom.*"
	             import="java.io.*"
	             import="javax.xml.transform.stream.*"
	             import="javax.xml.transform.dom.*"
	             import="javax.xml.xpath.*"
	             import="javax.servlet.jsp.JspWriter" %>
	<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
	<title>Sample ACS Filter</title>
	</head>
	<body>
		<h3>SAML information from sample ACS program</h3>
		<%!
	    void displaySAMLInfo(Node node, String parent, JspWriter out)
	    {
	    
		    try
		    {
				String nodeName;
			    int nChild, i;
			    
			    nodeName = node.getNodeName();
			    out.println("<br>");
			    out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");
			       
			       // Attributes.
			       NamedNodeMap attribsMap = node.getAttributes();
			       if (null != attribsMap)
			       {
	                     for (i=0; i < attribsMap.getLength(); i++)
	                     {
	                            Node attrib = attribsMap.item(i);
	                            out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
	                     }
			       }
			       
			       // Child nodes.
			       NodeList list = node.getChildNodes();
			       if (null != list)
	 		       {
			              nChild = list.getLength();
			              if (nChild > 0)
			              {                    
	
				                 // If it is a text node, just print the text.
				                 if (list.item(0).getNodeName() == "#text")
				                 {
	                                 out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
				                 }
				                 else
				                 {
				                	 // Print out the child node names.
				                	 out.print("Contains " + nChild + " child node(s): ");   
		   		                     for (i=0; i < nChild; i++)
				                     {
					                    Node temp = list.item(i);
					                    
					                    out.print("<b>" + temp.getNodeName() + "</b>");
					                    if (i < nChild - 1)
					                    {
					                    	// Separate the names.
					                    	out.print(", ");
					                    }
					                    else
					                    {
					                    	// Finish the sentence.
					                    	out.print(".");
					                    }
					                    	
				                     }
					                 out.println("<br>");
					                 
					                 // Process the child nodes.
					                 for (i=0; i < nChild; i++)
				                     {
					                    Node temp = list.item(i);
					                    displaySAMLInfo(temp, parent + nodeName + "\", out);
				                     }
				               }
			              }
			          }
			      }
			    catch (Exception e)
			    {
			    	System.out.println("Exception encountered.");
			    	e.printStackTrace();	    	
			    }
		    }
	    %>
	
	    <%
	    try 
	    {
		    String data  = (String) request.getAttribute("ACSSAML");
		    
		    DocumentBuilder docBuilder;
			Document doc = null;
			DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
			docBuilderFactory.setIgnoringElementContentWhitespace(true);
			docBuilder = docBuilderFactory.newDocumentBuilder();
			byte[] xmlDATA = data.getBytes();
			
			ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA); 
			doc = docBuilder.parse(in);
			doc.getDocumentElement().normalize();
			
			// Iterate the child nodes of the doc.
	        NodeList list = doc.getChildNodes();
	
	        for (int i=0; i < list.getLength(); i++)
	        {
	        	displaySAMLInfo(list.item(i), "", out);
	        }
		}
	    catch (Exception e) 
	    {
	    	out.println("Exception encountered.");
	    	e.printStackTrace();
		}
	    
	    %>
	</body>
	</html>

## Ausführen der Anwendung

1. Führen Sie Ihre Anwendung im Serveremulator oder als Bereitstellung in Azure aus. Führen Sie dazu die Schritte unter [Authentifizieren von Webbenutzern mit dem Azure Access Control Service über Eclipse][] aus.
2. Öffnen Sie Ihre Webanwendung in einem Browser. Nach der Anmeldung sehen Sie in Ihrer Anwendung die SAML-Informationen inklusive der Security Assertion des Identitätsanbieters.

## Nächste Schritte

Wenn Sie die ACS-Funktionalität genauer erforschen und mit anspruchsvolleren Szenarien experimentieren möchten, finden Sie weitere Informationen unter [Access Control Service 2.0][].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[Authentifizieren von Webbenutzern mit dem Azure Access Control Service über Eclipse]: ../active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
 

<!---HONumber=AcomDC_0309_2016-->