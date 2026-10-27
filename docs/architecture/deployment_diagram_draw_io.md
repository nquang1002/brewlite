<mxfile host="app.diagrams.net" modified="2026-10-28T00:00:00.000Z" agent="Mozilla/5.0" version="24.0.0" etag="brewlite-deployment" type="device">
  <diagram name="Deployment Diagram - BrewLite" id="deployment-diagram-id">
    <mxGraphModel dx="1422" dy="820" grid="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" page="1" pageScale="1" pageWidth="1169" pageHeight="827" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        
        <!-- Client Layer -->
        <mxCell id="box_client" value="Thiết bị người dùng (Client Device)" style="swimlane;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;fontColor=#333333;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="40" y="40" width="220" height="120" as="geometry" />
        </mxCell>
        <mxCell id="node_browser" value="Trình duyệt Web / PWA&#xa;(Next.js Client)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;" vertex="1" parent="box_client">
          <mxGeometry x="20" y="40" width="180" height="60" as="geometry" />
        </mxCell>

        <!-- CDN Layer -->
        <mxCell id="box_cdn" value="Cloudflare / CDN Edge" style="swimlane;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;fontColor=#333333;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="320" y="40" width="220" height="120" as="geometry" />
        </mxCell>
        <mxCell id="node_cdn" value="Static Assets Cache &amp;&#xa;HTTPS Termination" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffe6cc;strokeColor=#d79b00;" vertex="1" parent="box_cdn">
          <mxGeometry x="20" y="40" width="180" height="60" as="geometry" />
        </mxCell>

        <!-- Cloud Production Platform -->
        <mxCell id="box_cloud" value="Nền tảng Cloud Production (Render / Railway / VPS)" style="swimlane;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;fontColor=#333333;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="600" y="40" width="520" height="280" as="geometry" />
        </mxCell>
        
        <mxCell id="node_nextjs" value="Web Server Container&#xa;(Next.js SSR - Port 3000)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="box_cloud">
          <mxGeometry x="30" y="50" width="200" height="60" as="geometry" />
        </mxCell>

        <mxCell id="node_nestjs" value="API Server Container&#xa;(NestJS REST - Port 4000)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;" vertex="1" parent="box_cloud">
          <mxGeometry x="30" y="180" width="200" height="60" as="geometry" />
        </mxCell>

        <mxCell id="node_postgres" value="PostgreSQL 16&#xa;(Persistent Volume - 5432)" style="shape=cylinder3;whiteSpace=wrap;html=1;boundedLbl=1;backgroundIndex=1;fillColor=#e1d5e7;strokeColor=#9673a6;" vertex="1" parent="box_cloud">
          <mxGeometry x="290" y="170" width="190" height="80" as="geometry" />
        </mxCell>

        <!-- CI/CD Layer -->
        <mxCell id="box_cicd" value="Hệ thống CI/CD &amp; Registry" style="swimlane;whiteSpace=wrap;html=1;fillColor=#f5f5f5;strokeColor=#666666;fontColor=#333333;fontStyle=1;" vertex="1" parent="1">
          <mxGeometry x="600" y="380" width="520" height="150" as="geometry" />
        </mxCell>
        <mxCell id="node_github" value="GitHub Repository" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8cecc;strokeColor=#b85450;" vertex="1" parent="box_cicd">
          <mxGeometry x="30" y="50" width="130" height="60" as="geometry" />
        </mxCell>
        <mxCell id="node_actions" value="GitHub Actions&#xa;(Lint, Test, Build)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8cecc;strokeColor=#b85450;" vertex="1" parent="box_cicd">
          <mxGeometry x="190" y="50" width="130" height="60" as="geometry" />
        </mxCell>
        <mxCell id="node_registry" value="Container Registry&#xa;(Docker Hub / GHCR)" style="rounded=1;whiteSpace=wrap;html=1;fillColor=#f8cecc;strokeColor=#b85450;" vertex="1" parent="box_cicd">
          <mxGeometry x="350" y="50" width="140" height="60" as="geometry" />
        </mxCell>

        <!-- Connectors -->
        <mxCell id="conn_1" value="HTTPS" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source="node_browser" target="node_cdn">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="conn_2" value="HTTPS" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source="node_cdn" target="node_nextjs">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="conn_3" value="REST/JSON" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=0.5;exitY=1;entryX=0.5;entryY=0;" edge="1" parent="1" source="node_nextjs" target="node_nestjs">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="conn_4" value="Prisma ORM" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source="node_nestjs" target="node_postgres">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="conn_5" value="" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source="node_github" target="node_actions">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
        <mxCell id="conn_6" value="" style="edgeStyle=orthogonalEdgeStyle;rounded=0;html=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;" edge="1" parent="1" source="node_actions" target="node_registry">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>