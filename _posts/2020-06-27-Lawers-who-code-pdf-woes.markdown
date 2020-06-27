---
layout: post
title:  "Lawyers who code: PDF woes"
date:   2020-06-27 12:02:25 +0000
categories: other
---

![Header](https://richardbatstone.github.io/images/DD_012.PNG)

For the last few months I've been working with our professional support lawyer (PSL) team to refresh the look and form of one of our client publications. The project's not done yet so I won't describe it in detail but I wanted to share some know how about a thorny PDF problem in case it is helpful to others who come this way.

## The client publication

The client publication is a big ass PDF document that goes out to a bunch of corporate clients. It's basically a bible on a particular area of law: it's a high quality work product and a lot of lawyer hours go into producing it.

Since it's 2020, there's a feeling we can do a bit better than a static PDF document. We've spent the last couple of months talking to people (i.e. clients) who make use of the publication and we're now starting to get a sense of how we can improve it, both from a tech and a content perspective. Those conversations have been fascinating and I hope to be able to write about them too.

However, regardless of what other changes we make, there was one thing that the PSL team were absolutely adamant that we had to do: fix the endnote links.

## The problem

Functionally, the publication starts life as a really long Word document. It runs to hundreds of pages and a big part of it is made up of 'endnotes': additional information about things in the main body. There are hundreds of endnotes. When you are working in Word you can flick back and forth between the references in the document and the endnotes, but this functionality is lost when the document is converted to PDF.

So, right now, there's this publication that clients are using and if they want to read more about a topic they have to manually scroll to find the right endnote (which might be hundreds of pages away), and then remember their place so they can manually scroll back. You don't need to be a content designer to know that that sucks.

## This will be easy

"Of course", I said, grimly shaking my head. "As an absolute minimum, even if we do nothing else, we will fix these endnote links".

I was quite confident I could get this sorted out. Afterall, I am a _lawyer who codes_. I know all about machine learning and python and github and whatnot. This problem wasn't even going to need those skills, I just needed to get the right bit of software, or specify the right settings, to do the export properly.

My first port of call was to talk to our document specialist team who had helped put the publication together. Turns out they'd really tried to get them working last time but had hit a brick wall. Adobe Pro should do this 'out the box' but for some reason, on this document, it messes up the bookmarks. So you get links but they take you to the wrong place. Nothing the team did to the document seemed to change this, so they concluded the only option was to set the links manually and there wasn't time to do that.

Alright, so Adobe is a bit buggy: I'll see if we have better luck with another package.

I tried a _lot_ of different packages. Every one of them either:

 - failed to convert the endnote links; or
 - converted the links, but did a horrible job of handling the Word formatting.
 
I was getting a bit despondent by this point. Not only was it frustrating not to be able to solve what felt like a trivial problem, but I also felt I would be losing credibility with the project team: if I can't solve this, why would they trust me to steer the broader tech stuff?

*Sidenote: after much experimentation with Adobe, it seems the sticking points were (i) endnotes whose content was longer than 1 page (don't ask) and (ii) the splitting of endnotes so that they follow the relevant section rather than all at then end. But even if we adopted these constraints, thee links only worked in one direction.*

## The solution

One evening, one of the PSLs sent me a link to a [2014 discussion board talking about hyperlinking endnotes](https://answers.microsoft.com/en-us/office/forum/office_2013_release-word/hyperlink-footnotes-in-word-2013-so-they-will/525c58bf-6901-46dd-be45-a1b83dae893b). It sounded relevant but they weren't really sure what to do with it. The discussion included a macro that someone had written to set explicit hyperlinks between endnote (and footnote) references and their content. The idea is that it's then much easier to preserve the links when exporting to pdf.

I was fairly sceptical because: (i) I don't know any VBA; and (ii) I've copied enough buggy code from Stack Overflow to know it's rare to find a perfect solution, particularly from a 6 year old board.

However, I gave it a whirl. In case you haven't come across macros before, they're like little programs you can write to perform tasks inside of Word or Excel. You can create your own by clicking 'Macros' in the 'View' ribbon in Word.

Unsurprisingly, the macro didn't work when applied to our document. But it did work a bit. It just broke when it got to a certain endnote. This was encouraging enough for me to start poking around in the code (set out below) to see if I could identify a fix. Again, I know _nothing_ about VBA but I have spent a fair amount of time debugging things. I figured if I could identify the problem I could then try and google an answer.

A fews hours later I'd worked out that the problem occured when the endnote reference (the superscript number in the main body of the document) was part of bullet point text. So if you have a bullet point list and put an endnote on one of the points the macro would break. Our document is about 50% bullet points so we'd need a fix for this.

I started reading the docs for the functions related to placing the endnote references and on a hunch experimented with this line:

```
.Collapse wdCollapseStart
```

What if the collapsing direction was getting messed up when the reference is on a bullet point? I don't know. Like I say, I don't know anything about VBA. Anyway, I tried subbing in `.Collapse wdCollapseEnd` and BOOM, suddenly it runs fine. It loops through the document, turns the endnotes into hyperlinks and when you do a normal export to PDF, it all still works. 

It's quite possible I've broken the script or the document in some appalling way. But it doesn't matter. It works. Now I can make my PDF and deliver on that minimum ask.

*Original macro - all credit to Paul Edstein*

```
Option Explicit

Sub HyperlinkEndNotesFootNotes()
Dim SBar As Boolean           ' Status Bar flag
Dim TrkStatus As Boolean      ' Track Changes flag
Dim Rng1 As Range, Rng2 As Range, i As Long
' Store current Status Bar status, then switch on
SBar = Application.DisplayStatusBar
Application.DisplayStatusBar = True
' Store current Track Changes status, then switch off
With ActiveDocument
  TrkStatus = .TrackRevisions
  .TrackRevisions = False
End With
' Turn Off Screen Updating
Application.ScreenUpdating = False
With ActiveDocument

  'Process all endnotes
  For i = 1 To .Endnotes.Count
    'Give the OS a chance to do any background processing
    DoEvents
    'Update the statusbar
    StatusBar = "Processing Endnote " & i
    'Define two ranges: one to the endnote reference the other to the endnote content
    Set Rng1 = .Endnotes(i).Reference
    Set Rng2 = .Endnotes(i).Range.Paragraphs.First.Range
    With Rng1
      'Format the endnote reference as hidden text
      .Font.Hidden = True
      'Insert a number before the endnote reference and bookmark it
      .Collapse wdCollapseStart
      .Text = i
      .Style = "Endnote Reference"
      .Bookmarks.Add Name:="_ERef" & i, Range:=Rng1
    End With
    'Insert a number before the endnote content and bookmark it
    With Rng2
      'Format the endnote reference as hidden text
      With .Words.First
        If .Characters.Last Like "[ " & vbTab & "]" Then .End = .End - 1
        .Font.Hidden = True
      End With
      'Insert a number before the endnote reference and bookmark it
      .Collapse wdCollapseStart
      .Text = i
      .Style = " Endnote Reference"
      .Bookmarks.Add Name:="_ENum" & i, Range:=Rng2
    End With
    'Insert hyperlinks between the endnote references
    .Hyperlinks.Add Anchor:=Rng1, SubAddress:="_ENum" & i
    .Hyperlinks.Add Anchor:=Rng2, SubAddress:="_ERef" & i
    'Restore the Rng1 endnote reference bookmark
    .Bookmarks.Add Name:="_ERef" & i, Range:=Rng1
  Next
 
  'Process all footnotes
  For i = 1 To .Footnotes.Count
    'Give the OS a chance to do any background processing
    DoEvents
    'Update the statusbar
    StatusBar = "Processing Footnote " & i
    'Define two ranges: one to the footnote reference the other to the footnote content
    Set Rng1 = .Footnotes(i).Reference
    Set Rng2 = .Footnotes(i).Range.Paragraphs.First.Range
    With Rng1
      'Format the footnote reference as hidden text
      .Font.Hidden = True
      'Insert a number before the footnote reference and bookmark it
      .Collapse wdCollapseStart
      .Text = i
      .Style = "Footnote Reference"
      .Bookmarks.Add Name:="_FRef" & i, Range:=Rng1
    End With
    'Insert a number before the footnote content and bookmark it
    With Rng2
      'Format the footnote reference as hidden text
      With .Words.First
        If .Characters.Last Like "[ " & vbTab & "]" Then .End = .End - 1
        .Font.Hidden = True
      End With
      'Insert a number before the footnote reference and bookmark it
      .Collapse wdCollapseStart
      .Text = i
      .Style = " Footnote Reference"
      .Bookmarks.Add Name:="_FNum" & i, Range:=Rng2
    End With
    'Insert hyperlinks between the footnote references
    .Hyperlinks.Add Anchor:=Rng1, SubAddress:="_FNum" & i
    .Hyperlinks.Add Anchor:=Rng2, SubAddress:="_FRef" & i
    'Restore the Rng1 footnote reference bookmark
    .Bookmarks.Add Name:="_FRef" & i, Range:=Rng1
  Next
 
  'Update the statusbar
  StatusBar = "Finished Processing " & .Endnotes.Count & " Endnotes" & .Footnotes.Count & " Footnotes"
End With
Set Rng1 = Nothing: Set Rng2 = Nothing
' Restore original Status Bar status
Application.DisplayStatusBar = SBar
' Restore original Track Changes status
ActiveDocument.TrackRevisions = TrkStatus
' Restore Screen Updating
Application.ScreenUpdating = True
End Sub
```

## Lawyers who code

There are plenty of online voices discussing the benefits, the buzz and the bluster of law firms teaching lawyers to code. There are also some people doing really serious [work on the future of law and technology education](https://www.law.ox.ac.uk/research-and-subject-groups/ai-english-law-work-package-five). 

I would only add that, from a personal perspective, I have never regretted spending time trying to learn a new technology or programming language. And, if I hadn't spent time messing around in python, I doubt I would have had the confidence to try out the macro. I probably wouldn't have known where to start. That cuts both ways. I probably shouldn't have tried to fix the macro myself: I should have reached out to an expert in our tech team for help. But I'd much rather have the confidence to have a go than to not be able to engage at all.