import React, { useState } from "react";
import { DocumentUploader } from "@/components/DocumentUploader";
import { ResultsPanel } from "@/components/ResultsPanel";
import { Header } from "@/components/Header";
import { Footer } from "@/components/Footer";
import { DocumentList } from "@/components/DocumentList";
import { useToast } from "@/components/ui/use-toast";

export type DocumentType = 
  | "appraisal" 
  | "creditReport" 
  | "identification" 
  | "bankStatement" 
  | "application" 
  | "backgroundReport" 
  | "lease";

export type DocumentStatus = 
  | "eligible" 
  | "ineligible" 
  | "undetermined" 
  | "missing" 
  | "illegible" 
  | "badFormat" 
  | "flagged" 
  | "missingSignature" 
  | "processing" 
  | "notUploaded";

export interface DocumentInfo {
  type: DocumentType;
  displayName: string;
  status: DocumentStatus;
  file: File | null;
  details: string;
  requiresSignature: boolean;
}

const Index = () => {
  const { toast } = useToast();
  const [documents, setDocuments] = useState<DocumentInfo[]>([
    { type: "appraisal", displayName: "Appraisal", status: "notUploaded", file: null, details: "", requiresSignature: false },
    { type: "creditReport", displayName: "Credit Report", status: "notUploaded", file: null, details: "", requiresSignature: false },
    { type: "identification", displayName: "ID", status: "notUploaded", file: null, details: "", requiresSignature: false },
    { type: "bankStatement", displayName: "Bank Statement", status: "notUploaded", file: null, details: "", requiresSignature: false },
    { type: "application", displayName: "Application", status: "notUploaded", file: null, details: "", requiresSignature: true },
    { type: "backgroundReport", displayName: "Background Report", status: "notUploaded", file: null, details: "", requiresSignature: false },
    { type: "lease", displayName: "Lease", status: "notUploaded", file: null, details: "", requiresSignature: true },
  ]);
  
  const [resultsVisible, setResultsVisible] = useState(false);
  const [isAnalyzing, setIsAnalyzing] = useState(false);

  const handleDocumentUpload = (type: DocumentType, file: File) => {
    setDocuments(prevDocs => prevDocs.map(doc => 
      doc.type === type 
        ? { ...doc, file, status: "processing" } 
        : doc
    ));
    
    toast({
      title: "Document uploaded",
      description: `${file.name} has been uploaded successfully.`,
    });
    
    // Simulate processing (in a real app, this would call your N8N flow)
    simulateProcessing(type);
  };

  const simulateProcessing = (type: DocumentType) => {
    // In a real implementation, this would be replaced with a call to your API
    setTimeout(() => {
      const statuses: DocumentStatus[] = [
        "eligible", "ineligible", "undetermined", "missing", 
        "illegible", "badFormat", "flagged", "missingSignature"
      ];
      const randomStatus = statuses[Math.floor(Math.random() * statuses.length)];
      const randomDetails = getRandomDetailsForStatus(randomStatus);
      
      setDocuments(prevDocs => prevDocs.map(doc => 
        doc.type === type 
          ? { ...doc, status: randomStatus, details: randomDetails } 
          : doc
      ));
    }, 2000);
  };

  const getRandomDetailsForStatus = (status: DocumentStatus): string => {
    const detailsMap: Record<DocumentStatus, string[]> = {
      eligible: ["Meets all criteria; no flags or issues."],
      ineligible: ["Fails critical criteria.", "Has a disqualifying issue."],
      undetermined: ["Insufficient information provided.", "Conflicting information detected."],
      missing: ["Required documentation not provided."],
      illegible: ["Document cannot be read due to poor quality.", "Document is damaged."],
      badFormat: ["Document is corrupted.", "Document is improperly prepared.", "Document is in the wrong format."],
      flagged: ["Anomalies detected.", "Potential issues need further investigation."],
      missingSignature: ["Document is missing required signature."],
      processing: ["Processing document..."],
      notUploaded: ["Document not yet uploaded."]
    };
    
    const options = detailsMap[status] || ["Status unknown"];
    return options[Math.floor(Math.random() * options.length)];
  };

  const analyzeAllDocuments = () => {
    setIsAnalyzing(true);
    
    // Check if all documents are uploaded
    const allUploaded = documents.every(doc => doc.status !== "notUploaded");
    
    if (!allUploaded) {
      toast({
        title: "Missing documents",
        description: "Please upload all required documents before analyzing.",
        variant: "destructive"
      });
      setIsAnalyzing(false);
      return;
    }
    
    // Simulate API call delay
    setTimeout(() => {
      setResultsVisible(true);
      setIsAnalyzing(false);
    }, 3000);
  };

  const resetAnalysis = () => {
    setDocuments(prevDocs => prevDocs.map(doc => ({ 
      ...doc, 
      status: "notUploaded", 
      file: null, 
      details: "" 
    })));
    setResultsVisible(false);
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-950 to-green-900">
      <Header />
      
      <main className="container mx-auto px-4 py-8">
        <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
          <div className="lg:col-span-2">
            <h2 className="text-2xl font-semibold mb-6 text-blue-100">Document Upload</h2>
            <div className="grid grid-cols-1 md:grid-cols-2 gap-6">
              <DocumentList 
                documents={documents} 
                onUpload={handleDocumentUpload} 
              />
            </div>
            
            <div className="mt-8 flex flex-wrap gap-4">
              <button
                onClick={analyzeAllDocuments}
                disabled={isAnalyzing || documents.some(doc => doc.status === "processing")}
                className="shimmer px-6 py-3 bg-gradient-to-r from-blue-600 to-green-600 text-white rounded-lg hover:from-blue-700 hover:to-green-700 transition-all duration-300 disabled:from-blue-400 disabled:to-green-400 disabled:opacity-50 disabled:cursor-not-allowed"
              >
                {isAnalyzing ? "Analyzing..." : "Analyze Documents"}
              </button>
              <button
                onClick={resetAnalysis}
                className="px-6 py-3 bg-blue-900/30 backdrop-blur-sm text-blue-200 rounded-lg border border-blue-700/30 hover:bg-blue-800/40 transition-all duration-300"
              >
                Reset
              </button>
            </div>
          </div>
          
          <div className="lg:col-span-1">
            <ResultsPanel 
              documents={documents} 
              visible={resultsVisible} 
            />
          </div>
        </div>
      </main>
      
      <Footer />
    </div>
  );
};

export default Index;
