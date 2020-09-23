<div align="center">

## Menu pagelet for ASP\.NET


</div>

### Description

Simple server-side menu control - kind of - for ASP.NET pages. It's best suited for a site where all pages use the same menu. All pages share the same pagelet, which contains all the menu items and the logic. You can define two menu levels (main menu and submenus). When you click on a main menu item the submenu drops down. The currently shown page is shown bold in the menu.

The code contains the pagelet (navbar.ascx) and a sample page using it.
 
### More Info
 
The code uses VB.NET syntax but almost no dotnet features. You could adapt the code to work under plain ASP as well. The pagelet would become an include then. However, this is less elegant then using pagelets in ASP.NET.

You can see the code in action at www.klemid.de/aspmenu.aspx.

Maybe you need my styles.css to make it look good.


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Klemens Schmid \(old\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/klemens-schmid-old.md)
**Level**          |Beginner
**User Rating**    |4.6 (23 globes from 5 users)
**Compatibility**  |ASP\.NET
**Category**       |[Controls/ Forms/ Dialogs/ Menus](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/controls-forms-dialogs-menus__10-3.md)
**World**          |[\.Net \(C\#, VB\.net\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/net-c-vb-net.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/klemens-schmid-old-menu-pagelet-for-asp-net__10-253/archive/master.zip)





### Source Code

```
--------------------------------------------------
Pagelet code:
--------------------------------------------------
<!-- navigation bar for klemid's web
<script language="VB" runat="server">
	Dim MenuEntry(,) As object = { _
		{"Home", "welcome.aspx", "", true}, _
		{"Interests", "interests.aspx", "", true}, _
		{"My Tools", "tools.aspx", "", true}, _
		{"ASP.NET Menu", "aspmenu.aspx", "My Tools", false}, _
		{"SMS via http", "vbsms.aspx", "My Tools", false}, _
		{"OL2WAP", "ol2wap.aspx", "My Tools", false}, _
		{"Form Sniffer", "formsniffer2.aspx", "My Tools", false}, _
		{"Crypt File", "cryptfile.aspx", "My Tools", false}, _
		{"Copy Contacts", "copycontacts.aspx", "My Tools", false}, _
		{"eBay Reminder", "ebayrem.aspx", "My Tools", false}, _
		{"Fav2Web", "fav2web.aspx", "My Tools", false}, _
		{"Lex4VB", "lex4vb.aspx", "My Tools", false}, _
		{"M-Pad", "mpad.asp", "", true}, _
		{"My Charts", "mycharts.aspx", "", true}, _
		{"Select", "mycharts1.aspx", "My Charts", false}, _
		{"Display", "mycharts2.aspx", "My Charts", false}, _
		{"Urlaub", "urlaub.aspx", "", true}, _
		{"Ruhpolding", "ruhpolding.aspx", "Urlaub", false}, _
		{"Wiesloch", "wiesloch.aspx", "", true}, _
		{"Top 30 WapMarks", "topwap.aspx", "", true}, _
		{"Top 30 WebMarks", "topweb.aspx", "", true}, _
		{"All My WebMarks", "allweb.aspx", "", true}, _
		{"", "", "", true}, _
		{"Email", "mailto:feedback@schmidks.de?subject=Feedback", "", true}, _
		{"Disclaimer", "http://www.disclaimer.de/disclaimer.htm", "", true} _
	}
	Dim i as integer
	Dim strCurrentPage as String
	dim bSubmenuStarting as Boolean = false
	Sub Page_Load(Source as Object, E as EventArgs)
		Dim i, j as Integer
		Dim strParent as String
		'switch on the submenu according to the currently displayed page
		strCurrentPage = Request.Url.Segments.GetValue(Request.Url.Segments.GetUpperBound(0))
		For i=0 To MenuEntry.GetUpperBound(0)
			If MenuEntry(i, 1) = strCurrentPage Then
				If MenuEntry(i, 2) = "" Then
					'main menu item was clicked
					strParent = MenuEntry(i, 0)
				Else
					'submenu item was clicked
					strParent = MenuEntry(i,2)
				End If
				'found -> switch on the 'visible' flag for submenu items
				For j=0 To MenuEntry.GetUpperBound(0)
					If MenuEntry(j, 2) = strParent Then
						MenuEntry(j, 3) = True
					End If
				Next
				Exit For
			End If
		Next
	End Sub
</script>
-->
<table cellspacing="1" cellpadding="3" bgcolor="white" align=center>
	<% for i=0 to MenuEntry.GetUpperBound(0) %>
		<% if MenuEntry(i,0) = "" Then 'separator line%>
			<tr>
				<td bgcolor="#dedebe" height="1">
			</tr>
			<tr>
				<td bgcolor="#dedebe" height="1">
			</tr>
		<% else 'normal link %>
			<% if i > 0 AndAlso MenuEntry(i-1, 2) <> "" AndAlso MenuEntry(i, 2) = "" AndAlso MenuEntry(i-1, 3) Then 'submenu ends %>
				</table>
			<% end if %>
			<% bSubmenuStarting = (i > 0 AndAlso MenuEntry(i, 2) <> "" AndAlso MenuEntry(i, 3) AndAlso MenuEntry(i+1, 2) = "") %>
			<% if bSubmenuStarting Then %>
				<td bgcolor="#f2f2e6" >
					<table border="0" cellspacing="0" bgcolor="#ececd9">
			<% end if %>
			<% if MenuEntry(i, 2) <> "" Then 'submenu entry %>
				<% if MenuEntry(i, 3) Then %>
					<tr>
						<td width="100%" bgcolor="#f2f2e6" <%= IIf(MenuEntry(i,1)=strCurrentPage, "style=""FONT-WEIGHT: bold""", "") %> >
							<font size="2"><nobr>
							<a href="<%= MenuEntry(i,1) %>" class="NavLink"><%= MenuEntry(i,0)%></a>
							</nobr></font>
						</td>
					</tr>
				<% End if%>
			<% else 'main menu %>
				<tr>
					<td bgcolor="#dedebe" height="18" <%= IIf(MenuEntry(i,1)=strCurrentPage, "style=""FONT-WEIGHT: bold""", "") %> >
						<nobr>
						<a href="<%= MenuEntry(i,1) %>" class="NavLink"><%= MenuEntry(i,0)%></a>
						</nobr>
					</td>
				</tr>
			<% end if %>
		<% end if %>
	<% next %>
</table>
--------------------------------------------------
Sample page
--------------------------------------------------
<%@ Register TagPrefix="klemid" TagName="NavBar" Src="navbar.ascx" %>
<%@ Page Language="vb" AutoEventWireup="true" Debug="true" %>
<%@ Import namespace="System.IO" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.0 Transitional//EN">
<HTML>
	<HEAD>
		<title>ASP.NET Menu</title>
		<meta content="Microsoft Visual Studio.NET 7.0" name="GENERATOR">
		<meta content="Visual Basic 7.0" name="CODE_LANGUAGE">
		<meta http-equiv="Content-Type" content="text/html; charset=windows-1252">
		<meta http-equiv="Content-Language" content="en-us">
		<meta content="JavaScript" name="vs_defaultClientScript">
		<meta content="http://schemas.microsoft.com/intellisense/ie5" name="vs_targetSchema">
		<LINK href="Styles.css" type="text/css" rel="stylesheet">
		<script id="fill" runat="server">
			Sub Page_Load(sender as object, e as System.EventArgs)
			Dim SReadToEnd As Stream
			'read the pagelet
			SReadToEnd = File.OpenRead(Page.MapPath(".\navbar.ascx"))
			Dim SrReadToEnd As StreamReader = New StreamReader(SReadToEnd, _
				System.Text.Encoding.ASCII)
			'SrReadToEnd.BaseStream.Seek(0, SeekOrigin.Begin)
			txtPagelet.Text = SrReadToEnd.ReadToEnd()
			SrReadToEnd.Close()
			'read this page
			SReadToEnd = File.OpenRead(Page.MapPath(Page.Request.FilePath()))
			SrReadToEnd = New StreamReader(SReadToEnd, System.Text.Encoding.ASCII)
			'SrReadToEnd.BaseStream.Seek(0, SeekOrigin.Begin)
			txtThisPage.Text = SrReadToEnd.ReadToEnd()
			SrReadToEnd.Close()
			End Sub
		</script>
	</HEAD>
	<body style="BACKGROUND-IMAGE: none" leftMargin="0" topMargin="0" marginheight="0" marginwidth="0">
		<table dir="ltr" height="100%" cellSpacing="0" cellPadding="10" width="100%" border="0">
			<tr>
				<td vAlign="top" bgColor="#cccc99">
					<!-- header --><klemid:navbar id="NavBar" runat="server"></klemid:navbar></td>
				<td vAlign="top" width="100%">
					<!-- actual body goes here --><a name="Oben"></a>
					<h2 align="center">ASP.NET Menu</h2>
					<p>
					<p style="MARGIN-TOP: 10px; MARGIN-BOTTOM: 5px" align="center">
					Inspect the source code of the menu seen on the left side
					<P>
					<P>
						<hr>
						See how the menu on the left side of my pages is implemented. It is done by an
						ASP.NET pagelet. Of course, you can buy a full-blown ASP.NET menu control like
						the ones found at <A href="http://www.asp.net">www.asp.net</A>
					, but those are not free and they are mostly overkill for a small web site
					having only one menu.
					<P></P>
					<P>Although the code here works only for ASP.NET it would be an easy exercise the
						adapt it for pure ASP.
					</P>
					<form runat="server">
						<P></P>
						<P>Pagelet code (navbar.ascx):</P>
						<P>
							<asp:TextBox id="txtPagelet" runat="server" Width="100%" Height="114px" TextMode="MultiLine" ReadOnly="True"></asp:TextBox></P>
						<P>This page's code:</P>
						<p></p>
						<asp:TextBox id="txtThisPage" runat="server" Width="100%" Height="114px" TextMode="MultiLine" DESIGNTIMEDRAGDROP="282" ReadOnly="True"></asp:TextBox>
						<P></P>
						<P>&nbsp;</P>
					</form>
				</td>
			</tr>
		</table>
	</body>
</HTML>
```

